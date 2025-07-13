---
title: Hello World - 用Github pages和Hexo搭建自己的个人主页
date: 2022-02-24 10:54:57
categories: 搭建环境
tags:
- git
- 搭建环境
  
top: True
---

**Hello world**
欢迎来到我的博客，我是Kennan，一名计算机专业的研究生。以前在不同的地方写一些文章博客，但会受到平台的各种限制和广告。
这里在前辈的指导下，通过 [github pages平台](https://docs.github.com/en/pages) 和 [Hexo博客框架](https://hexo.io/) 搭建了一个自己的个人主页，在上面分享一些自己的经历和学习心得。
下面记录一下我的搭建过程，请多多指教，一些相关文件可参考我的 [KennanYang.github.io](https://github.com/KennanYang/KennanYang.github.io) 项目
<!--more-->
##  1.搭建Hexo
[Hexo](https://hexo.io/)是基于Node.js写的，也需要git管理文章上传到github，所以需要先安装git和nodeJS
### 安装git
windows：到git官网上下载,[Download git](https://gitforwindows.org/)
linux: 

```bash
sudo apt-get install git
```

使用`git --version`查看是否安装正确
### 安装Node.js
windows：[Node.js官网](https://nodejs.org/en/)选择LTS版本（稳定版）。

linux：
```bash
sudo apt-get install nodejs
sudo apt-get install npm
```
使用`node -v`和`npm -v`查看是否安装正确
### 安装Hexo
创建一个文件夹【filename】(我的叫 hexoblog)，然后`cd`到这个文件夹下
```bash
npm install -g hexo-cli
```
用`hexo -v`查看一下版本

至此，安装完毕，开始配置Hexo项目
### 配置Hexo
初始化Hexo

```bash
hexo init hexoblog
cd hexoblog //进入这个hexoblog文件夹
npm install
```
然后就可以查看官方的demo了

```bash
hexo generate //产生网页，可缩写hexo g
hexo server //挂到本地服务器打开，可缩写hexo s
```

在浏览器输入localhost:4000就可以看到你生成的博客，官方默认主题是landscope

## 2.部署到github pages
直接在github page平台上托管我们的博客，便于维护，下面是把Hexo搭好的博客部署到github pages的配置方式。
### 注册github，新建repo
新建一个自己用户名命名的仓库，后面加.github.io，像我这样，其他设置默认就好，点击create repository。
![创建repo](https://img-blog.csdnimg.cn/c5bdfacd70c94789bcbe12c7a1c01da9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6LSwS2VubmFu,size_20,color_FFFFFF,t_70,g_se,x_16)
### 生成SSH添加到GitHub
回到你的git bash中，

```bash
git config --global user.name "yourname"
git config --global user.email "youremail"
```

然后创建SSH,一路回车

```bash
ssh-keygen -t rsa -C "youremail"
```
这个时候它会告诉你已经生成了.ssh的文件夹。在你的电脑中找到这个文件夹。
![ssh密钥](https://img-blog.csdnimg.cn/1b4a2ce586c5423c91354dd38ecee893.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6LSwS2VubmFu,size_20,color_FFFFFF,t_70,g_se,x_16)
ssh，简单来讲，就是一个秘钥，其中，id_rsa是你这台电脑的私人秘钥，不能给别人看的，id_rsa.pub是公共秘钥，可以随便给别人看。把这个公钥放在GitHub上，这样当你链接GitHub自己的账户时，它就会根据公钥匹配你的私钥，当能够相互匹配时，才能够顺利的通过git上传你的文件到GitHub上。
![github->settings](https://img-blog.csdnimg.cn/51f61bbe5dc64632be0158105eb15d3b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6LSwS2VubmFu,size_11,color_FFFFFF,t_70,g_se,x_5)

而后在GitHub的setting中，找到SSH keys的设置选项，点击`New SSH key`把你的`id_rsa.pub`里面的信息复制进去。
![在这里插入图片描述](https://img-blog.csdnimg.cn/b3cb64a9604946a38fc3f8bd85ce4f6a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6LSwS2VubmFu,size_20,color_FFFFFF,t_70,g_se,x_16)

在gitbash中，查看是否成功
```bash
ssh -T git@github.com
```
### 部署Hexo到github
打开站点配置文件 _config.yml，翻到最后，修改为
```bash
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```
这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```bash
npm install hexo-deployer-git --save
```
然后
```bash
hexo clean
hexo generate
hexo deploy
```

其中 `hexo clean`清除了你之前生成的东西，也可以不加。
`hexo generate` 是生成静态文章，可以用 `hexo g`缩写
`hexo deploy` 部署文章，可以用`hexo d`缩写

注意deploy时可能要你输入username和password。

部署后需要**等待一段时间**，然后就可以在`http://yourname.github.io`看到Hexo 博客了，这里的内容和`hexo server`生成的内容完全相同。
## 3.绑定个人域名
完成上面的步骤后，可以使用`http://yourname.github.io`查看个人主页，如何自定义一个属于自己的域名呢？
### 购买域名
注册一个[阿里云账户](https://www.aliyun.com/?spm=5176.100251.top-nav.dlogo.5af94f152mfbDz),在阿里云上买一个域名，我买的是`kennan-yang.top`

先实名认证，然后在域名控制台添加解析，这里需要**等半天时间**。
![域名解析](https://img-blog.csdnimg.cn/05356c75bdfa48d593e1789995afe188.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6LSwS2VubmFu,size_20,color_FFFFFF,t_70,g_se,x_16)
登录GitHub，进入之前创建的仓库`yourname.github.io`，点击`settings->pages`，设置`Custom domain`，输入你的域名`kennan-yang.top` 并`save`。
![Custom domain](https://img-blog.csdnimg.cn/4ca6ad5c655b4eaab7f534ed8cc78ab7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN6LSwS2VubmFu,size_20,color_FFFFFF,t_70,g_se,x_16)
### 绑定域名
然后在你的博客文件夹（如我的hexoblog）的`source`目录中创建一个名为`CNAME`文件，不要后缀。写上你的域名。
![CNAME](https://img-blog.csdnimg.cn/e26efd4edb2a4772a54ded2542a28b14.png)
然后就是最常用的下面几条命令，当配置完成之后进行这些操作即可。
```bash
hexo clean # 清理缓存
hexo g # hexo generate 生成静态页
hexo s # hexo server 本地预览（非必须）
hexo d # hexo deploy 部署到github pages
```
部署完成后就可以用你的域名打开自己的博客啦！
# 参考资料
Hexo还有更多不同的主题和配置，可参考下面的资料进行个性化设置。
1. [CSDN: hexo史上最全搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029)
 2. [github pages](https://docs.github.com/en/pages) 
 3. [Hexo官方文档](https://hexo.io/)
 4. [Hexo的岛主题](https://shen-yu.gitee.io/)