
# DetectNet_v2

相比分类任务而言，检测任务的配置文件需要关注更多的内容。`TLT`以`DetectNet_v2`为例详细讲解了相应的组件配置，然后对不同的算法进行特定配置

## 组件概述

配置文件的内容包含了数据、模型、训练和评估，根据处理流程按以下顺序学习：

1. 模型配置
2. 数据扩充
3. 数据加载
4. 损失函数
5. 训练配置
6. 输出处理
7. 验证配置

##  模型配置

在其他目标检测算法中会制定各自模型的配置，不过这里使用了`model_config`，*嗯。。。是自家出品的*

```
# Sample model config for to instantiate a resnet18 model with pretrained weights and freeze blocks 0, 1
# with all shortcuts having projection layers.
model_config {
  arch: "resnet"
  pretrained_model_file: <path_to_model_file>
  freeze_blocks: 0
  freeze_blocks: 1
  all_projections: True
  num_layers: 18
  use_pooling: False
  use_batch_norm: True
  dropout_rate: 0.0
  training_precision: {
    backend_floatx: FLOAT32
  }
  objective_set: {
    cov {}
    bbox {
      scale: 35.0
      offset: 0.5
    }
  }
}
```

* `use_batch_norm`：是否使用批量归一化层。`True/False`
* `dropout_rate`：随机失活。默认为`0.0`，设置范围为`[0.0, 0.1]`
* `training_precision`：训练精度
        * `backend_floatx`：默认为`FLOAT32`
* `objective_set`：定义网络目标，当前这部分不需要修改

## 数据扩充

数据扩充模型包含`3`个部分，分别是输入图像/边界框处理、空间扩充以及颜色扩充

```
# Sample augementation config for 
augmentation_config {
  preprocessing {
    output_image_width: 960
    output_image_height: 544
    output_image_channel: 3
    min_bbox_width: 1.0
    min_bbox_height: 1.0
  }
  spatial_augmentation {

    hflip_probability: 0.5
    vflip_probability: 0.0
    zoom_min: 1.0
    zoom_max: 1.0
    translate_max_x: 8.0
    translate_max_y: 8.0
  }
  color_augmentation {
    color_shift_stddev: 0.0
    hue_rotation_max: 25.0
    saturation_shift_max: 0.2
    contrast_scale_max: 0.1
    contrast_center: 0.5
  }
}
```

* `augmentation_config`：缩放输入图像到统一宽/高，过滤极小边界框
    * `output_image_width`：缩放图像宽。匹配模型输入的高，必须是`16`的倍数，并且`>480`
    * `output_image_height`：缩放图像高。匹配模型输入的高，必须是`16`的倍数，并且`>272`
    * `output_image_channel`：图像通道数。匹配模型输入的通道数，可选`1/3`
    * `min_bbox_width`：过滤边界框的宽
    * `min_bbox_height`：过滤边界框的高
* `spatial_augmentation`
    * `hflip_probability`：水平翻转概率
    * `vflip_probability`：垂直翻转概率
* `color_augmentation`
    * `color_shift_stddev`：颜色偏移的标准偏差值。取值`[0.0, 1.0]`
    * `hue_rotation_max`：色调旋转矩阵的最大旋转角度。取值`[0.0, 1.0]`
    * `saturation_shift_max`：改变饱和度的最大位移。默认值`0.2`
    * `contrast_scale_max`：给定中心点旋转时对比度的斜率。默认值`0.1`
    * `contrast_center`：对比度旋转中心。理想情况下，该值设置为最大像素值的一半。（因为输入图像在[0, 1.0]之间缩放，所以可以将此值设置为0.5）
  
## 数据加载

```
dataset_config {
  data_sources: {
    tfrecords_path: "<path to the training tfrecords root/tfrecords train pattern>"
    image_directory_path: "<path to the training data source>"
  }
  image_extension: "jpg"
  target_class_mapping {
      key: "car"
      value: "car"
  }
  target_class_mapping {
      key: "automobile"
      value: "car"
  }
  target_class_mapping {
      key: "heavy_truck"
      value: "car"
  }
  target_class_mapping {
      key: "person"
      value: "pedestrian"
  }
  target_class_mapping {
      key: "rider"
      value: "cyclist"
  }
  validation_fold: 0
}
```

* `data_sources`：数据路径
    * `tfrecords_path`：tfrecords文件所在目录
    * `image_directory_path`：图像所在目录
* `image_extension`：图像扩展名
* `target_class_mapping`：指定了输入边界框类别与训练目标类别，比如`car/automibile/heavy_truck`都可以属于`car`
  
## 损失函数

保持不变就好了

## 训练配置

```
training_config {
  batch_size_per_gpu: 16
  num_epochs: 80
  learning_rate {
    soft_start_annealing_schedule {
      min_learning_rate: 5e-6
      max_learning_rate: 5e-4
      soft_start: 0.1
      annealing: 0.7
    }
  }
  regularizer {
    type: L1
    weight: 3e-9
  }
  optimizer {
    adam {
      epsilon: 1e-08
      beta1: 0.9
      beta2: 0.999
    }
  }
  cost_scaling {
    enabled: False
    initial_exponent: 20.0
    increment: 0.005
    decrement: 1.0
  }
}
```

