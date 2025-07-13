---
title: 如何连接实验室的服务器进行网络训练？
date: 2022-02-24 18:48:44
categories: 搭建环境
tags: 
- 服务器
- linux
---
我在用电脑训练CNN时遇到了性能瓶颈（显存不够），当得知实验室的服务器算力更强时，去请教师兄怎么连。

实验室的师兄甩过来服务器的ip和用户名密码，说直接连就行。

直接连？linux都不太会用的小白，咋连？

首先确保连接到实验室的网络，我这边是用校园网或者挂校园网的VPN，然后有下面几种配置方法（方法应该很多，只是列出了我尝试过的这三种）：
<!--more-->
# 方法一：wsl（Windows Subsystem for Linux）
1.下载wsl2
参考
[https://docs.microsoft.com/zh-cn/windows/wsl/install-win10](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)

2.下载windows终端windows terminal（非必须，也可直接用power shell）
[https://docs.microsoft.com/zh-cn/windows/terminal/get-started](https://docs.microsoft.com/zh-cn/windows/terminal/get-started)

3.使用ssh命令进行外部链接

```bash
-> ssh 用户@ip
-> 密码
```

# 方法二：vscode
1.vscode下载remote-ssh和remote wsl
![](https://img-blog.csdnimg.cn/20210519110928571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)

2.连接服务器

选择左下角的标志
![](https://img-blog.csdnimg.cn/20210519111221622.png)
会弹出一个菜单栏，选择Connect to Host...，输入用户名和密码即可
![](https://img-blog.csdnimg.cn/20210519111159132.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODA1NTk3,size_16,color_FFFFFF,t_70)

# 方法三（推荐）：使用终端模拟器XShell和XFtp，学校和家庭有免费版
[https://www.netsarang.com/zh/free-for-home-school/](https://www.netsarang.com/zh/free-for-home-school/)
![](https://img-blog.csdnimg.cn/2021051911143257.png)

XShell用来输入指令

XFtp方便文件管理和传输

当连接完成之后，就可以把使用GPU的网络训练代码放在服务器上跑了。。
