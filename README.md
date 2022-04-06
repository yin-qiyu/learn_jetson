# learn_jetson

<div id="top"></div>

**此仓库为jetson nano环境配置+模型加速部署的笔记**



1. [Jetson nano 环境配置+模型部署](https://github.com/yin-qiyu/learn_jetson/blob/master/1.Environment%20configuration.md)
2. [jetson nano环境搭建+yolov5+tensorrt+deepstream](https://github.com/yin-qiyu/learn_jetson/blob/master/2.%20yolov5%2Btensorrt%2Bdeepstream.md)
2. [mask detection](https://github.com/yin-qiyu/learn_jetson/blob/master/3.%E5%8F%A3%E7%BD%A9%E6%A3%80%E6%B5%8Bdemo.md)

> - 经过测试转成tensorrt后推理速度大幅加快
> - 640*640图片 Yolov5.s单张推理速度在130ms左右
> - 转换成tensorrt后的单张推理速度在70ms左右
> - nano模型推理速度更是能到30ms左右

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220310143041733.png" alt="image-20220310143041733" width="500"  />

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220310143135601.png" alt="image-20220310143135601" width="500" />

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220405210138122.png" alt="image-20220405210138122" width="200" />



- csi摄像头推理速度在25帧左右

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/CE28BCCF64EA67A5B0B092D36EBB79B2.jpg" alt="CE28BCCF64EA67A5B0B092D36EBB79B2" width="500"  />

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220227180343701.png" alt="image-20220227180343701" width="700" />





