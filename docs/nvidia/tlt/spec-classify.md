
# 分类任务

分类任务只需要配置`3`个模块即可

1. 模型配置
2. 训练配置
3. 评估配置

## 模型配置

```
model_config {

  # Model architecture can be chosen from:
  # ['resnet', 'vgg', 'googlenet', 'alexnet', 'mobilenet_v1', 'mobilenet_v2', 'squeezenet', 'darknet', 'googlenet']

  arch: "resnet"

  # for resnet --> n_layers can be [10, 18, 34, 50, 101]
  # for vgg --> n_layers can be [16, 19]
  # for darknet --> n_layers can be [19, 53]


  n_layers: 18
  use_bias: True
  use_batch_norm: True
  all_projections: True
  use_pooling: False
  freeze_bn: False
  freeze_blocks: 0
  freeze_blocks: 1

  # image size should be "3, X, Y", where X,Y >= 16
  input_image_size: "3,224,224"
}
```

* `arch`：模型架构，比如`resnet/vgg/alexnet/googlenet/mobilenet_v1/v2/squeezenet/darknet`
* `n_layers`：同一模型架构中可能存在多个变体，比如`resent-18/34/50/101`等
* `use_bias`：是否使用偏置值
* `all_projections`：在残差结构中，是否对所有快捷连接使用$1\times 1$卷积进行过滤
* `use_pooling`：下采样过程中，使用`max pooling（最大池化）`还是`strided convolution（跨步卷积）`。如果为`true`，则使用最大池化方法；如果为`false`，则使用跨步卷积方法。**`Nvidia`推荐使用跨步卷积方式**
* `freeze_bn`：是否保留预训练模型的`BN`层参数不变
* `freeze_blocks`：保留预训练模型的哪个块的参数不变
    * 对于`resnet`系列，可选数值为`[0, 1, 2, 3]`
    * 对于`vggnet`系列，可选数值为`[1, 2, 3, 4, 5]`
    * 对于`mobilenet-v1`，可选数值为`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`
    * 对于`mobilenet-v2`，可选数值为`[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]`
    * 对于`googlenet`，可选数值为`[0, 1, 2, 3, 4, 5, 6, 7]`
* `input_image_size`：输入图像大小

## 训练配置

```
train_config {
  train_dataset_path: "/path/to/your/train/data"
  val_dataset_path: "/path/to/your/val/data"
  pretrained_model_path: "/path/to/your/pretrained/model"
  # optimizer can be chosen from ['adam', 'sgd']

  optimizer: "sgd"
  batch_size_per_gpu: 256
  n_epochs: 80
  n_workers: 16

  # regularizer
  reg_config {
    type: "L2"
    scope: "Conv2D,Dense"
    weight_decay: 0.00005

  }

  # learning_rate

  lr_config {

    # "step" and "soft_anneal" are supported.

    scheduler: "soft_anneal"

    # "soft_anneal" stands for soft annealing learning rate scheduler.
    # the following 4 parameters should be specified if "soft_anneal" is used.
    learning_rate: 0.005
    soft_start: 0.056
    annealing_points: "0.3, 0.6, 0.8"
    annealing_divider: 10
    # "step" stands for step learning rate scheduler.
    # the following 3 parameters should be specified if "step" is used.
    # learning_rate: 0.006
    # step_size: 10
    # gamma: 0.1
    
    # "cosine" stands for soft start cosine learning rate scheduler.
    # the following 2 parameters should be specified if "cosine" is used.
    # learning_rate: 0.05
    # soft_start: 0.01

  }
}
```

* `train_config`：指定了训练过程中的学习率调度、正则化、优化器以及数据和模型的路径
    * `train_dataset_path`：训练数据集所在目录
    * `val_dataset_path`：验证数据集所在目录
    * `pretrained_model_path`：预训练模型所在目录
    * `optimizer`：优化器，可选`sgd`或者`adam`
    * `batch_size_per_gpu`：每个`gpu`的批量大小
    * `n_epochs`：迭代次数
    * `n_workers`：工作线程
    * `reg_config`：配置了正则化器相关参数
        * `type`：正则化方法，可选`L1/L2/None`
        * `scope`：适用于哪种类型层
        * `weight_decay`：权重衰减率
    * `lr_config`：学习率调度

## 评估配置

```
eval_config {
  eval_dataset_path: "/path/to/your/eval/data"
  model_path: "/path/to/your/model"
  top_k: 3
  conf_threshold: 0.5
  batch_size: 256
  n_workers: 8
}
```

* `eval_dataset_path`：测试集所在目录
* `model_path`：训练模型路径
* `top_k`：取前`k`个结果进行判断
* `conf_threshold`：置信度阈值
* `batch_size`：批量操作大小
* `n_workers`：工作线程

## 示例一：ResNet

```
model_config {
  arch: "resnet",
  n_layers: 18
  # Setting these parameters to true to match the template downloaded from NGC.
  use_batch_norm: true
  all_projections: true
  freeze_blocks: 0
  freeze_blocks: 1
  input_image_size: "3,224,224"
}
train_config {
  train_dataset_path: "/workspace/tlt-experiments/data/split/train"
  val_dataset_path: "/workspace/tlt-experiments/data/split/val"
  pretrained_model_path: "/workspace/tlt-experiments/classification/pretrained_resnet18/tlt_pretrained_classification_vresnet18/resnet_18.hdf5"
  optimizer: "sgd"
  batch_size_per_gpu: 64
  n_epochs: 80
  n_workers: 16

  # regularizer
  reg_config {
    type: "L2"
    scope: "Conv2D,Dense"
    weight_decay: 0.00005
  }

  # learning_rate
  lr_config {
    scheduler: "step"
    learning_rate: 0.006
    #soft_start: 0.056
    #annealing_points: "0.3, 0.6, 0.8"
    #annealing_divider: 10
    step_size: 10
    gamma: 0.1
  }
}
eval_config {
  eval_dataset_path: "/workspace/tlt-experiments/data/split/test"
  model_path: "/workspace/tlt-experiments/classification/output/weights/resnet_080.tlt"
  top_k: 3
  batch_size: 256
  n_workers: 8
}
```