---
title: 'hexo配置过程中遇到的问题汇总'
date: 2022-06-15 13:19:41
categories: 搭建环境
tags:
- 搭建环境
---
在hexo配置个人主页的过程中，遇到一些环境配置问题，在这里整理一下大神们的解决办法：

1.主页打不开的问题：

打开github主页时发现**404**，通过 ***hexo server*** 命令打开本地的主页配置时发现 Cannot Get/,所以搜索了一下解决办法，最终参考[vue项目Error: Cannot find module ‘xxx’类报错的解决方法](https://blog.csdn.net/loveLifeLoveCoding/article/details/121789466) 解决了主页打不开的问题。

<!--more-->
2.数学公式问题：

这个问题是当我根据marktown写数学公式时，发现vscode上显示公式正确，但是配置到主页上时，公式就变成了源码，通过学习这个博客 [hexo博客next主题添加对数学公式的支持](https://blog.csdn.net/weixin_45511189/article/details/115798563)，得到解决。

marktown的数学公式语法规则为：[Marktown数学公式](https://www.jianshu.com/p/f0de9f572c9d)

