

# 介绍

在主机上训练自己的Yolov5模型，转为TensorRT模型并部署到Jetson Nano上，用DeepStream运行。（先以yolov5s.pt为例）

# 环境

硬件环境：

- 带cuda的显卡主机

- Jetson Nano 4G B01

- csi摄像头、usb摄像头

软件环境：

- yolov5-5.0
- jetpack-4.4
- deepstream-5.0
- Tensorrt-7.1
- Cuda-10.2

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220227222344603.png" alt="image-20220227222344603" style="zoom: 25%;" />

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220227222435137.png" alt="image-20220227222435137" style="zoom: 25%;" />

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220227222508150.png" alt="image-20220227222508150" style="zoom:50%;" />





# 电脑上

- 下载yolov5

```bash
$ git clone -b v5.0 https://github.com/ultralytics/yolov5.git
$ git clone -b yolov5-v5.0 https://github.com/wang-xinyu/tensorrtx.git
```

资料：

[tensorrtx/yolov5 at yolov5-v5.0 · wang-xinyu/tensorrtx (github.com)](https://github.com/wang-xinyu/tensorrtx/tree/yolov5-v5.0/yolov5)





