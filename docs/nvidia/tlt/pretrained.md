
# 准备预训练模型

必须从[Nvidia NGC](https://ngc.nvidia.com)官网上下载预训练模型，需要注册账户并获取`API_KEY`。`ngc`提供了命令行工具用于预训练模型的查询和下载，参考：[NGC Catalog CLI](https://docs.nvidia.com/ngc/ngc-catalog-cli-user-guide/index.html)

下面记录如下使用`NGC CLI`工具下载预训练模型，实现步骤如下：

1. 配置API_KEY
2. 查询模型列表
3. 下载模型

## 安装NGC CLI

参考：[Installing NGC Catalog CLI](https://docs.nvidia.com/ngc/ngc-catalog-cli-user-guide/index.html)

## 配置API

```
ngc config set 
Enter API key [no-apikey]. Choices: [<VALID_APIKEY>, 'no-apikey']: <your-api-key>
Enter the CLI output format type. Choices: [ascii, csv, json]: <your-output-format-type> 
```

## 查询模型列表

```
ngc registry model list <model_glob_string>
```

比如查询所有预训练模型

```
$ ./ngc registry model list nvidia/tlt_pretrained_*
[{
    "application": "Classification",
    "description": "Transfer learning toolkit compatible image classification pre-trained models to help accelerate the AI training process and eliminate the time consuming process of training from scratch.",
    "displayName": "TLT Image Classification",
    "framework": "Transfer Learning Toolkit",
    "isPublic": true,
    "labels": [
        {
            "key": "general",
            "values": [
                "Deep learning",
                "transfer learning",
                "image classification",
                "intelligent video analytics",
                "computer vision",
                "manufacturing",
                "smart cities",
                "transfer learning toolkit",
                "TLT",
                "retail",
                "healthcare"
            ]
        },
        {
            "key": "framework",
            "values": [
                "Transfer Learning Toolkit"
            ]
        },
        {
            "key": "precision",
            "values": [
                "FP32"
            ]
        },
        {
            "key": "publisher",
            "values": [
                "NVIDIA"
            ]
        }
    ],
    "latestVersionIdStr": "mobilenet_v1",
    "latestVersionSizeInBytes": 27489360,
    "modelFormat": "TLT",
    "name": "tlt_pretrained_classification",
    "orgName": "nvidia",
    "precision": "FP32",
    "publisher": "NVIDIA",
    "updatedDate": "2020-05-06T18:24:30.069Z"
},{
    "application": "Object Detection",
    "description": "Transfer learning toolkit compatible object detection models for YOLOV3, FasterRCNN, SSD, DSSD and RetinaNet to help accelerate the AI training process and eliminate the time consuming process of training from scratch.",
    "displayName": "TLT Object Detection",
    "framework": "Transfer Learning Toolkit",
    "isPublic": true,
    "labels": [
        {
            "key": "general",
            "values": [
                "transfer learning",
                "image classification",
                "intelligent video analytics",
                "manufacturing inspection",
                "object detection",
...
...
```

查询所有的分类模型

```
$ ./ngc registry model list nvidia/tlt_pretrained_classification:*
[{
    "accuracyReached": 74.38,
    "batchSize": 1,
    "createdByUser": "iaka0m3mg9c91ltqg3a85obt9o",
    "createdDate": "2020-04-29T20:27:01.709Z",
    "description": "",
    "gpuModel": "V100",
    "memoryFootprint": "38.3",
    "numberOfEpochs": 80,
    "status": "UPLOAD_COMPLETE",
    "totalFileCount": 1,
    "totalSizeInBytes": 40173128,
    "versionId": "resnet10"
},{
    "accuracyReached": 76.74,
    "batchSize": 1,
    "createdByUser": "iaka0m3mg9c91ltqg3a85obt9o",
    "createdDate": "2020-04-29T20:26:17.872Z",
    "description": "",
    "gpuModel": "V100",
    "memoryFootprint": "89.0",
    "numberOfEpochs": 80,
    "status": "UPLOAD_COMPLETE",
    "totalFileCount": 1,
    "totalSizeInBytes": 93278448,
    "versionId": "resnet18"
},{
    "accuracyReached": 77.04,
    "batchSize": 1,
    "createdByUser": "iaka0m3mg9c91ltqg3a85obt9o",
    "createdDate": "2020-04-29T20:25:14.042Z",
    "description": "",
    "gpuModel": "V100",
...
...
```

## 下载模型

```
ngc registry model download-version <ORG/model_name:version> -dest <path_to_download_dir>
```

比如下载模型`resnet18`

```
ngc registry model download-version nvidia/tlt_pretrained_classification:resnet18 --dest pretrained_resnet18
```