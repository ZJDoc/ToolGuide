
# 安装

## 硬件环境

* `Minimum`

```
4 GB system RAM
4 GB of GPU RAM
Single core CPU
1 GPU
50 GB of HDD space
```

*` Recommended`

```
32 GB system RAM
32 GB of GPU RAM
8 core CPU
4 GPUs
100 GB of SSD space
```

## 软件环境


* `Ubuntu 18.04 LTS`
* `NVIDIA GPU Cloud account and API key` - https://ngc.nvidia.com/
* [docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [nvidia-docker2](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))
* `NVIDIA GPU driver v410.xx or above`

注意：推荐使用[DeepStream 5.0](https://developer.nvidia.com/deepstream-sdk)

## 安装

```
# 登录账户
$ docker login nvcr.io
Username: $oauthtoken               -- 就输这个就好了
Password: API_KEY                          -- 替换成NGC的api-key
#下载容器
docker pull nvcr.io/nvidia/tlt-streamanalytics:<version>
```

*最新版本容器参考：[Transfer Learning Toolkit for Video Streaming](https://ngc.nvidia.com/catalog/containers/nvidia:tlt-streamanalytics)*

## Jupyter Notebook

在容器内部，`TLT`提供了`jupyter notebook`文件，用于详细说明整体训练过程

```
# 启动容器，开放8888端口
$ docker run --runtime=nvidia -it -v /home/<username>/tlt-experiments:/workspace/tlt-experiments -p 8888:8888 nvcr.io/nvidia/tlt-streamanalytics:<version>
# 进入/workspace/examples文件夹，启动jupyter notebook
$ jupyter notebook --ip 0.0.0.0 --allow-root
```

*Note：修改`<username>`和`<version>`*