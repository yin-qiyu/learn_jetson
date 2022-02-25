# 1. jupyter import torch 报错

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





 

## 2. Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-*解决办法

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



# 2. sd卡烧录太慢

换一个好一点的读卡器速度就起来了



<img src="https://raw.githubusercontent.com/yin-qiyu/picbed/master/img/image-20220222141902969.png" alt="image-20220222141902969" width="400" />





# 删除opt下的文件

```bash
rm -rf filename
```



# apt下载失败

下载过程如果因为网络原因失败的话可以在命令后加上 -i https://pypi.tuna.tsinghua.edu.cn/simple 来使用清华镜像源

