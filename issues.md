目录

- [jupyter import torch 报错](#jupyter-import-torch-报错)
- [failed with error code 1 in /tmp/pip-build-*](#failed-with-error-code-1-in-tmppip-build-)
- [删除opt下的文件](#删除opt下的文件)
- [apt下载失败](#apt下载失败)
- ['SPPF'问题](#sppf问题)
- [vnc分辨率设置](#vnc分辨率设置)
- [多摄像头无法启动问题](#多摄像头无法启动问题)
- [部署自己的模型出错](#部署自己的模型出错)
- [darknet调用csi](#darknet调用csi)
- [存储器扩容](#存储器扩容)
- [备份系统](#备份系统)
- [darknet调用csi绿屏](#darknet调用csi绿屏)
- [海康摄像头花屏](#海康摄像头花屏)




#  jupyter import torch 报错

```bash
OSError:/usr/lib/aarch64-linux-gnu/libgomp.so.1: cannot allocate memory in static TLS block
```

搜索下方的报错信息不难找到解决方法，大概有如下几种：

1.修改`import cv2`的位置，将其放在`import tensorflow`以及`import keras`之前。或者放在程序的最前面，试过放在程序的最前面没有问题。

2.在运行`jupyter notebook/Lab`之前引入环境变量：

```text
export LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1
```

这其中要注意，如果你的 jupyter 被设置为开机启动（我想大多数同学都会为了方便把它设为开机启动，并且监听来自所有 IP 的连接），那么在之后引入的环境变量对之前启动的 jupyter 是无效的。

根据中外网友的反映，方案1，2都可能有效，但在笔者的测试过程中，只有方案2有效，且每次启动jupyter前都要运行一遍，十分的麻烦。但好在笔者注意到了官方人员的一个回复：

![image-20220222141839089](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220222141839089.png )





原来 jupyter 会开启另一个进程去执行 Python 代码，**而我们当前引入的环境变量实际上只对当前进程有效**，也就是说每一次引入的环境变量在关闭当前的SSH连接（物理机上就是当前终端窗口）后即失效了，所以每次运行jupyter notebook前都需要引入一次环境变量，是否有更方便的解决方法呢？

上文中提到，我们常常会把jupyter通过service的方式设置为开机启动，如果在开机启动时即引入环境变量，这样不就可以完美解决我们的问题了吗？

4. 最终解决方案

如果你已经把 jupyter 设置为了开机启动，那么在 `/etc/systemd/system` 中应该会有一个.service文件（如果没有可以自己百度教程设置一下），把它用**sudo权限**打开后内容如下：



<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220222141848364.png" alt="image-20220222141848364" width="800" />





在[Service]条目中添加如下语句：

```text
Environment="LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1"
```

保存后退出编辑，重启就完成了设置，这样在每次开机启动jupyter时都会自动引入环境变量，也就不用手动输入了

测试一下：

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220222141854183.png" alt="image-20220222141854183" width="800"/>





一切正常，大功告成。





 

#  failed with error code 1 in /tmp/pip-build-*

我在使用pip3 install paramiko 的时候，出现了报错

```bash
...
        raise DistutilsError("Setup script exited with %s" % (v.args[0],))
    distutils.errors.DistutilsError: Setup script exited with error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
    
    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-23ykqx51/pynacl/ 
```

百度了好久也试了前人的很多方法，最后终于找到了答案。

```bash
pip3 install --upgrade pip
```

然后再执行pip3 install paramiko，然没有报错安装成功了 。







# 删除opt下的文件

```bash
rm -rf filename
```



# apt下载失败

下载过程如果因为网络原因失败的话可以在命令后加上 -i https://pypi.tuna.tsinghua.edu.cn/simple 来使用清华镜像源





#  'SPPF'问题

[(7条消息) Can‘t get attribute ‘SPPF‘ on ＜module ‘models.common‘ from ‘C:\\Users\\dell\\Desktop\\yolov5-5.0\\mo_RookiChen的博客-CSDN博客](https://blog.csdn.net/RooKichenn/article/details/120866650)



# vnc分辨率设置

[jetson nano 分辨率](https://blog.csdn.net/xiangfengbingzhi/article/details/119703356?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.pc_relevant_paycolumn_v3&spm=1001.2101.3001.4242.1&utm_relevant_index=3)

应用程序->启动应用程序中添加启动应用程序

名称随意

xrandr --fb 1024x600

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220307130253917.png" alt="image-20220307130253917" width="600" />

# 多摄像头无法启动问题

[DeepStream4 Jetson nano 多摄像头问题 - 智能视频分析 / DeepStream SDK - NVIDIA 开发者论坛](https://forums.developer.nvidia.com/t/deepstream4-jetson-nano-multiple-webcams-issue/79030)



# 部署自己的模型出错

- 报错：

```bash
ERROR from primary_gie: Failed to create NvDsInferContext instance
Debug info: /dvs/git/dirty/git-master_linux/deepstream/sdk/src/gst-plugins/gst-nvinfer/gstnvinfer.cpp(809): gst_nvinfer_start (): /GstPipeline:pipeline/GstBin:primary_gie_bin/GstNvInfer:primary_gie:
Config file path: /home/yqy/workspace/Yolov5-mask-detect-in-Deepstream-5.0-tensorrt7/Deepstream 5.0/config_infer_primary_yoloV5.txt, NvDsInfer Error: NVDSINFER_CONFIG_FAILED
App run failed
```

- 解决：





# darknet调用csi

[(7条消息) jetson-nano项目：使用csi摄像头运行yolov3-tiny demo_x16516581的博客-CSDN博客_csi摄像头 jetson nano](https://blog.csdn.net/x16516581/article/details/100570038)



# 存储器扩容

1.打开虚拟机(Ubuntu 18.04)的终端，输入以下命令安装扩容软件

sudo apt install gparted

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264666720520-20220303213343533.png) 

2.在虚拟机的系统应用内搜索并打开gparted

<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220228195911196.png" alt="image-20220228195911196" style="zoom:25%;" /> 

3.选择U盘的盘符，切记这几不能选错。例如我这里选择/dev/sdb，可以看到/sdb1后面有一部分是白色和一部分是灰色。

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264672744049-20220303213345080.png) 

4.右键选择Unmount来卸载/dev/sdb1，

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264675462158.png) 

5.右键选择Check检查U盘，点击上面的勾‘√’确认，再点击Apply。

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264678560241-20220228195940915-20220303213346062.png)

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264682834350-20220303213346530.png) 

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264685906494-20220303213347042.png) 

6.选择Resize/Move修改容量大小，直接将右边白色部分拖到最右边，再点击Resize/Move。

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264688738464-20220303213347575.png) 

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264692516357-20220303213348078.png) 

7.点击上面的勾‘√’确认，再点击Apply.

 

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264695929779-20220303213348562.png) 

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264698567932-20220303213351835.png) 

8.到此U盘扩容完成。

![image.png](https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/1643264702429809-20220303213352507.png) 

# 备份系统

[Clone SD Card – Jetson Nano and Xavier NX – JetsonHacks](https://www.jetsonhacks.com/2020/08/08/clone-sd-card-jetson-nano-and-xavier-nx/)

[为树莓派制作系统镜像时进行瘦身，方便后续保存与批量写入|树莓派|tf卡|fdisk|linux|磁盘_手机网易网 (163.com)](https://3g.163.com/dy/article/GP684MJV0552NFB7.html)







# darknet调用csi绿屏

[解决Jetson Nano使用CSI摄像头在Darknet下实时检测绿屏_/*wywy*/的博客-CSDN博客](https://blog.csdn.net/qq_44360908/article/details/122777848)





# 海康摄像头花屏

改udp连接
