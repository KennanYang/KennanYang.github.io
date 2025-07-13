---
title: win10安装python3.9.1+cuda11.1+cudnn+pytorch+opencv记录
date: 2022-02-26 18:54:57
categories: python
tags: python
---
最近由于机器学习大作业需要用到神经网络，记录一下配置环境的过程。
前人铺路，我只是结合自己的环境做了一个简单的总结，写的不好多多见谅。
<!--more-->
# 1.安装python3.9
首先是python的安装，选用当前时间最新版的python3.9.1
官网下载安装包即可[https://www.python.org/downloads/](https://www.python.org/downloads/)
![](https://img-blog.csdnimg.cn/20201219134620158.png)

一路“下一步”，建议安装到默认路径。

然后，配置环境变量：
找到安装的位置，把图中所示的两个路径加入环境变量。
我的路径是：
C:\Users\Administrator\AppData\Local\Programs\Python\Python39
C:\Users\Administrator\AppData\Local\Programs\Python\Python39\Scripts
![](https://img-blog.csdnimg.cn/20201219134237283.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)
补充一点：
conda里python中的pytorch比较老，遇到没法调用的问题（这里没有深究），所以没有用anaconda，而是直接官网找最新的python。
# 2.安装cuda（需要vs环境，我的是vs2015）

CUDA（Compute Unified Device Architecture），是显卡厂商NVIDIA推出的运算平台。

我当前的环境是vs2015，电脑配置是

![](https://img-blog.csdnimg.cn/20201219142426303.png)
![](https://img-blog.csdnimg.cn/20201219142440290.png)
注意CUDA的版本不能超过GPU的版本。

官网的CUDA安装路径如下：
[https://developer.nvidia.com/zh-cn/cuda-downloads](https://developer.nvidia.com/zh-cn/cuda-downloads)
![](https://img-blog.csdnimg.cn/20201219142239715.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)
圈出来的部分，前者是一个比较大的安装包，下载到本地安装，后者是很小的安装包，但是需要联网，我选的前者。
![](https://img-blog.csdnimg.cn/20201219142349445.png)

仍然选择当前最新的版本11.1，安装到默认路径下：
C:\Program Files\NVIDIA GPU Computing Toolkit

# 3.安装cudnn（版本和cuda对应，更老的版本也可以但不建议）
NVIDIA cuDNN是用于深度神经网络的GPU加速库。
老规矩，走官网。
[https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)
![](https://img-blog.csdnimg.cn/20201219140906756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)

这里的下载需要注册，有个技巧是复制链接，在迅雷打开可以直接下载。
![](https://img-blog.csdnimg.cn/20201219135916291.png)

下载好的压缩包解压之后，会发现三个文件夹。
分别把文件放到cuda路径对应的文件夹下。
![](https://img-blog.csdnimg.cn/20201219135443329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)
# 4.安装pytorch
它是一个基于Python的可续计算包，提供两个高级功能：1、具有强大的GPU加速的张量计算（如NumPy）。2、包含自动求导系统的深度神经网络。
官网：
[https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)
![](https://img-blog.csdnimg.cn/20201219140414305.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)
可以通过pip安装，最下面的命令行在cmd中打开即可。
由于速度慢，所以直接从别人那里拷贝过来了。

可以在python中通过下面的代码来检验，True表示pytorch的cuda配置成功
![](https://img-blog.csdnimg.cn/20201219142901202.png)

# 5.安装opencv
方法一：
打开cmd，输入`pip install opencv-python`即可，但超级慢还可能断开连接。
此处建议清华镜像下载

```bash
pip install opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

方法二：
下载相应Python版本的OpenCV的whl文件
https://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv
然后打开cmd，在whl文件对应文件夹路径下，使用pip安装，如
![](https://img-blog.csdnimg.cn/20201219141255475.png)

## 至此我需要的环境已经配置完成。
