---
title: HTTP协议详解
date: 2022-04-30 10:46:33
categories: 计算机网络
tags:
- 计算机网络
---

HTTP协议是网络编程的基础知识，后端开发应当熟练掌握，但是相关内容较多。

之前在学校学的都忘记了，最近为了找工作又复习起来，参考了力扣上的教程和一些网上资料，做一个总结，并补充一些常问的八股方便回顾。

[校招基础知识详解-HTTP](https://leetcode.cn/leetbook/read/tech-interview-cookbook/o9ga26/)


<!--more-->
## URL解析过程
当输入网址时会发生什么呢？提到HTTP就不能不先了解一下URL的解析过程。

### URL

在WWW上，每一信息资源都有统一的且在网上唯一的地址，该地址就叫**URL（Uniform Resource Locator,统一资源定位器）**，它是WWW的统一资源定位标志，就是指网络地址。

### 浏览器输入地址后发生什么？

1、域名解析： 浏览器获得URL地址，向操作系统请求该URL对应的IP地址，操作系统查询DNS（首先查询本地HOST文件，没有则查询网络）获得对应的IP地址

解释：

把URL分割成几个部分：协议、网络地址、资源路径

协议：指从该计算机获取资源的方式，常见的是HTTP、FTP

网络地址：可以是域名或者是IP地址，也可以包括端口号，如果不注明端口号，默认是80端口

如果地址不是一个IP地址，则需要通过DNS（域名系统）将该地址解析成IP地址，IP地址对应着网络上的一台计算机，DNS服务器本身也有IP，你的网络设置包含DNS服务器的IP，例如，www.abc.com不是一个IP，则需要向DNS询问请求www.abc.com对应的IP，获得IP，在这个过程中，你的电脑直接询问DNS服务器可能没有发现www.abc.com对应的IP，就会向它的上级服务器询问，这样依次一层层向上级找，最高可达根节点，直到找到或者全部找不到为止

端口号就相当于银行的窗口，不同的窗口负责不同的服务，如果输入www.abc.com:8080/，则表示不使用默认的80端口，而使用指定的8080端口

2、确认好了IP和端口号，则可以向该IP地址对应的服务器的该端口号发起TCP连接请求

3、服务器接收到TCP连接请求后，回复可以连接请求，

4、浏览器收到回传的数据后，还会向服务器发送数据包，表示三次握手结束

5、三次握手成功后，开始通讯，根据HTTP协议的要求，组织一个请求的数据包，里面包含请求的资源路径、你的身份信息等。

例如，www.abc.com/images/1/表示的资源路径是images/1/，发送后，服务器响应请求，将数据返回给浏览器，数据可以是根据HTML协议组织的网页，里面包含页面的布局、文字等等，也可以是图片或者脚本程序等，如果资源路径指定的资源不存在，服务器就会返回404错误，如果返回的是一个页面，则根据页面里的一些外链URL地址，重复上述步骤，再次获取

6、渲染页面，并开始响应用户的操作

7、窗口关闭时，浏览器终止与服务器的连接



## HTTP基础


**超文本传输协议（Hyper Text Transfer Protocol，HTTP）** 是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。

关于HTTP1.1协议的具体内容可以参考[RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.2.2)。 


### 1. 报文格式
客户端发送一个请求报文给服务器，服务器根据请求报文中的信息进行处理，并将处理结果放入响应报文中返回给客户端。

如下图所示方式可打开HTTP报文查看内容。
[http协议以及如何在谷歌控制台查看通信报文](https://blog.csdn.net/x1037490413/article/details/117704060)

上方为请求报文解析内容，下方为响应报文内容。

![图1. 一个HTTP报文](https://pic.imgdb.cn/item/629440300947543129839d01.png)

**1.1 请求报文格式如下：**

+ 第一行是包含了请求方法、URL、协议版本；
+ 接下来的多行都是请求首部 Header，每个首部都有一个首部名称，以及对应的值。
+ 一个空行用来分隔首部和内容主体 Body
+ 最后是请求的内容主体

```http
GET http://www.example.com/ HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: max-age=0

作者：CyC2018
链接：https://leetcode.cn/leetbook/read/tech-interview-cookbook/orse6h/
来源：力扣（LeetCode）
```


**1.2 应答报文格式如下：**

+ 第一行包含协议版本、状态码以及描述，最常见的是 200 OK 表示请求成功了，如图1
+ 接下来多行也是首部内容
+ 一个空行分隔首部和内容主体
+ 最后是响应的内容主体

### 2. HTTP 方法

**GET**

获取资源

**HEAD**

获取报文首部，和 GET 方法类似，但是不返回报文实体主体部分。

主要用于确认 URL 的有效性以及资源更新的日期时间等。

**POST**

传输实体主体

POST 主要用来传输数据，而 GET 主要用来获取资源。

<font color = #888888>

*GET和POST的区别？*
1)	后退按钮或刷新，Get无害，post数据会被重新提交；
2)	Get所使用的URL可以被设置为书签，而post不可以；
3)	Get能够被缓存，而post不可以；
4)	Get参数保留在浏览器历史中，而post参数不会保留在浏览器历史中；
5)	当发生数据时，get方法向URL添加数据，URL的数据长度是受限的，而post没有数据长度限制；
6)	Get只允许ASCII编码，而post没有限制；
7)	Get安全性没有post安全性好；
8)	Get数据在URL中对所有人是可见的，而在post中数据不会显示在URL中。
9)	Get产生一个TCP数据包，post产生两个TCP数据包；对于get方式的请求，浏览器会把header和data一并发送出去；对于post，浏览器先发送header再发送data；
10)	GET和POST本质上就是TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。

</font>


**PUT**

上传文件

由于自身不带验证机制，任何人都可以上传文件，因此存在安全性问题，一般不使用该方法。

**PATCH**

对资源进行部分修改

PUT 也可以用于修改资源，但是只能完全替代原始资源，PATCH 允许部分修改。

**DELETE**

删除文件

与 PUT 功能相反，并且同样不带验证机制。

**OPTIONS**

查询支持的方法

查询指定的 URL 能够支持的方法。

会返回 Allow: GET, POST, HEAD, OPTIONS 这样的内容。

**CONNECT**

要求在与代理服务器通信时建立隧道

使用 SSL（Secure Sockets Layer，安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

**TRACE**

追踪路径

服务器会将通信路径返回给客户端。

发送请求时，在 Max-Forwards 首部字段中填入数值，每经过一个服务器就会减 1，当数值为 0 时就停止传输。

通常不会使用 TRACE，并且它容易受到 XST 攻击（Cross-Site Tracing，跨站追踪）。

### HTTP 状态码

服务器返回的 **响应报文** 中第一行为状态行，包含了状态码以及原因短语，用来告知客户端请求的结果。

|状态码	|类别	|含义
|:-:|:-:|:-:|
|1XX	|Informational（信息性状态码）	|接收的请求正在处理
|2XX	|Success（成功状态码）	|请求正常处理完毕
|3XX	|Redirection（重定向状态码）	|需要进行附加操作以完成请求
|4XX	|Client Error（客户端错误状态码）	|服务器无法处理请求
|5XX	|Server Error（服务器错误状态码）	|服务器处理请求出错

**1. 1XX 信息**

100 Continue ：表明到目前为止都很正常，客户端可以继续发送请求或者忽略这个响应。

**2. 2XX 成功**

200 OK

204 No Content ：请求已经成功处理，但是返回的响应报文不包含实体的主体部分。一般在只需要从客户端往服务器发送信息，而不需要返回数据时使用。

206 Partial Content ：表示客户端进行了范围请求，响应报文包含由 Content-Range 指定范围的实体内容。

**3. 3XX 重定向**

301 Moved Permanently ：永久性重定向

302 Found ：临时性重定向

303 See Other ：和 302 有着相同的功能，但是 303 明确要求客户端应该采用 GET 方法获取资源。

**4. 4XX 客户端错误**

400 Bad Request ：请求报文中存在语法错误。

401 Unauthorized ：该状态码表示发送的请求需要有认证信息（BASIC 认证、DIGEST 认证）。如果之前已进行过一次请求，则表示用户认证失败。

403 Forbidden ：请求被拒绝。

404 Not Found

**5. 5XX 服务器错误**

500 Internal Server Error ：服务器正在执行请求时发生错误。

503 Service Unavailable ：服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。

## 参考资料
[1.百度百科-HTTP](https://baike.baidu.com/item/HTTP/243074?fr=aladdin)

[2.校招基础知识详解-HTTP](https://leetcode.cn/leetbook/read/tech-interview-cookbook/o9ga26/)

[3.rfc2616：3.2.2 http URL](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.2.2)

[4.http协议以及如何在谷歌控制台查看通信报文](https://blog.csdn.net/x1037490413/article/details/117704060)