---
title: http2.0——速度与激情
date: 2018-06-04 11:28:02
categories: "开发"
tags:
	- Nginx
	- Chrome
	- CPU
	- HTML
	- OpenSSL

---

摘要: 目前提升H5应用加载速度的方式有很多，比如缓存、cdn加速、代码压缩合并和图片压缩等技术。这些方法相信大伙如数家珍，然而这些招用完后，是否还有优化空间呢？现在我们祭出大杀器——HTTP 2.0 ...

背景

目前提升H5应用加载速度的方式有很多，比如缓存、cdn�����速、代码压缩合并和图片压缩等技术。这些方法相信大伙如数家珍，然而这些招用完后，是否还有优化空间呢？现在我们祭出大杀器——HTTP 2.0

相信大家听说过HTTP 2.0,那么它和HTTP 1.X相比效果到底如何呢？请看疗效：

单次页面加载速度对比

实践出真知，有请DEMO先生出场——一个网页，加载了8张图片，图片大小合计10MB

 *  http 1.x：

DOMContentLoaded: 215ms, Load: 549ms

![http2.0��—��度与激情][http2.0]

 *  http2.0:

DOMContentLoaded:156ms, Load:491ms

![http2.0——速度与激情][http2.0 1]

从数据中大家可以看到，对于这个DEMO页，HTTP 1.X需要花费549ms，而使用HTTP 2.0则可以减少60ms左右的等待。

Emmmm……才60ms,貌似没想像中改善那么大，60ms用户应该无感吧？

确实对单次用户访问来说，改善不大，但是用户浏览一个网站，不是一次就结束了，他会在接下来的时间里与网���产生���次互动。如果每个请求我们都能节省一点点时间，那么在用户的多次互动中，累加起来的时间应该比较可观了。

若是在大并发请求下，效果又当如何呢？上！压！测！

压测数据对比

模拟1,000,000 次请求压测数据如下：

http不同协议请求参数对比

请求协议类型 请求花费时间(s) 每秒请求数 每一请求花费时间 交换速率(kb/s) http1.1 144.035 6942.75 0.864 7328.87 https-spdy 128.133 7804.36 0.769 8238.4 https 145.883 6854.82 0.875 7236.05 https with h2 116.13 8611.28 0.686 8140.8 从上表可以看到http2.0和http1.1相比，节省30s左右的时间，没有对比没有伤害。

（压测方法可见文末附录二）

网络表现不错，我们再来看看服务器资源消耗情况。

cpu 使用情况

我们将相关数据制成了图表，方便大家比对

us: 用户空间占用cpu时间

sy：系统空间占用cpu时间

hi： 硬盘中断

si： 软件中断

http1.1

![http2.0——速度与激情][http2.0 2]

cpu(us+sy)平均：82.51%

http2.0

![http2.0——速度与激情][http2.0 3]

cpu(us+sy)平均：63.09%

疗效相当不错，看官们是否动心了呢？接下来为你呈上运用之道。

http 协议发展

![http2.0——速度与激情][http2.0 4]

使用http2.0的必备条件

默认https支持http2.0（2015年提出）

浏览器支持ALPN

查看是否支持http2.0的三种方法

使用openssl测试

终端输入：echo | openssl s\_client -alpn h2 –connect 域名:443

![http2.0——速度与激情][http2.0 5]

通过Chrome浏览器的属性查看

输入框键入 chrome://net-internals/\#http2

![http2.0——速度与激情][http2.0 6]

通过Chrome的debug工具查看

protocol为h2

![http2.0——速度与激情][http2.0 7]

http1.1与http2.0的区别

协议类型 传输格式 多路复用 报头压缩 服务端主动推送 请求优先级 http1.1 文本 X X X X http2.0 二进制帧 √ √ √ √ 报头压缩

通过h2load 命令访问

![http2.0——速度与激情][http2.0 8]

多路复用

http1.1

http1.1 without pipelining: 通过tcp连接上一个请求相应完后，下一个请求才能发出

http1.1 with pipelining: 通过tcp连接，上一个请求发出，下一个请求不需要等待，但是返回是同一顺序。

![http2.0——速度与激情][http2.0 9]

HTTP 1.X的网络加载情况，同一域名，最多支持6个Tcp连接，加载效果如同“正三角形”，看得我强迫症都犯了……

http2.0

在TCP连接上传输的是帧，客户端会将要传输的数据拆分为不同的帧，并标记对应的数据流ID，异步发出，服务端接收到帧集合根据数据流ID拼凑起来即为客户端发送来的数据。同理，服务端也是将数据拆分为不同帧返回。

每个请求通过tcp连接发出不需要等待其他请求返回， 所有的数据返回没有顺序。

![http2.0——速度与激情][http2.0 10]

HTTP 2.0的网络加载���况，同一���刻，可以发出30以上的数据帧且无序，感觉很舒服有没有？？

我们不妨以货物运输来再现http1.1与http2.0的请求场景。

A地与C地之间相隔一条河流，货物运输需要途径桥梁。假定现有横跨A，C两地的6座桥梁（类似Chrome同一域名最大支持6个TCP通道），以其中一座双向路线桥梁b1为例，

http1.1的过程：

一辆运输车辆从A地出发，通过桥梁b1，到达C地，然而只有在该��辆从C地取货并���回后，另一辆车才能出发。如此有序往返...

http2.0的过程：

所有的车辆无序上桥，取货后返回，根据车辆牌照卸载对应取货物品。

显而易见，第二种方式，桥梁利用率高，车辆运输货物多。

请求优先级

http1.1 不支持请求优先级，在统一域名大量请求同时发送时，浏览器支持6个请求，由于没有优先级，加载的时候会出现javascript资源阻塞情况。

http2.0， 浏览器通过headers和priority携带优先级信息，服务器生成优先级树，指导资源的分配（内存，cpu时间，带宽）。

优先级最高： 主要的html

优先级高： CSS文件

优先级中： js文件

优先级低： 图片

服务端主动推送

http1.1：在访问页面时，浏览器识别html页面上的link链接，然后请求对应的资源，浏览器开始识别到请求需要一定时间。

http2.0: 在访问页面时，服务器主动推动资源下���，让浏览器直接请���link资源。

目前nginx1.13.9版本支持push功能

附录一： nginx安装配置http2.0

软件要求

nginx 版本1.9.5以上 nginx 下载页

openssl 1.0.2以上 openssl 下载页

安装过程

``````````
//下载nginx/1.10.3 source包tar xvgf nginx-1.10.3.tar.gzcd nginx-1.10.3./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-openssl=/usr/local/openssl-1.0.2h --with-http_v2_modulemake -j4make install
``````````

配置http2.0

listen 443 ssl http2;

``````````
...http { include mime.types; default_type application/octet-stream; sendfile on; #tcp_nopush on; keepalive_timeout 65; gzip on; server { listen 80; server_name 172.16.37.66; location / { root html; index index.html index.htm; } } # HTTPS server # server { listen 443 ssl http2; server_name 172.16.37.66; ssl_certificate certs/cert.crt; ssl_certificate_key certs/cert.key; ssl_session_cache shared:SSL:1m; ssl_session_timeout 5m; ssl_ciphers HIGH:!aNULL:!MD5; ssl_prefer_server_ciphers on; location / { root html; index index.html index.htm; } }}
``````````

附录二：压测

压测工具

对于http,https使用apache 的ab

对于http/2使用nghttp2的h2load

压测http

``````````
ab -k -t 180 -c 6 -n 1000000 http://172.16.37.66/index.html
``````````

![http2.0——速度与激情][http2.0 11]

压测http2.0

http2.0之压测

``````````
h2load -c 6 -T 180 -n 1000000 https://172.16.37.66/index.html
``````````

![http2.0——速度与激情][http2.0 12]

附��三：http协议

定义

Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议

组成

请求行+请求头+请求正文+返回头部+返回正文

请求行

method Request-URI Http Version

GET /index.html HTTP/1.1

请求头部

``````````
Host: 172.16.37.66Cache-Control: max-age=0Upgrade-Insecure-Requests: 1User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8Accept-Encoding: gzip, deflateAccept-Language: zh-CN,zh;q=0.9,en;q=0.8If-None-Match: "5aae859e-34b"If-Modified-Since: Sun, 18 Mar 2018 15:28:30 GMT
``````````

请求正文

请求是form表单数据或application/json 格式数据

返回头部

``````````
Server: nginx/1.10.3Date: Wed, 21 Mar 2018 21:26:19 GMTLast-Modified: Sun, 18 Mar 2018 15:28:30 GMTConnection: keep-aliveETag: "5aae859e-34b"Access-Control-Allow-Origin: *
``````````

返回正文

服务端返回的结果

TCP握手

SYN---->SYN-ACK---->ACK

附录四： https协议

定义

https是一个在网络上改写的超文本安全通信传输协议。

在原有的TCP三次握手基础上，还有一次握手服务端下发ssl证书（所有者，域名公钥，数字签名，证书时间等），客户端验证证书。

证书

自签名证书或CA颁发

加密

对称加密和非对称加密

附���五：SPDY与ALPN

SPDY ：是speedy的缩写，由Google开发实现，现已被弃用，它的目的是减少页面加载延迟，提高网站安全，主要是通过压缩，多路复用，优先级减少延迟。

ALPN：Application-Layer Protocol Negotiation, 它允许应用层商议在一个安全的连接上那种协议执行，为了避免循回绕行。

在与http2.0一起使用时，可以提高页面压缩率，减少网络延迟。

作者简介：

就职于甜橙金融信息技���部，负责前端开发工作，先后任职携程，平安等公司的前端开发工程师职位，喜欢研究新的技术，服务于业务需求，熟练运用Linux/Unix系统命令，以终端为载体，快速执行操作。


[http2.0]: /pro/os/crawler/Z3AR-2MBA-UEBM.jpg
[http2.0 1]: /pro/os/crawler/YYRI-ZVYA-6VYZ.jpg
[http2.0 2]: /pro/os/crawler/E7FU-NMUR-AIRY.jpg
[http2.0 3]: /pro/os/crawler/N3EZ-FIYB-JVJY.jpg
[http2.0 4]: /pro/os/crawler/JIM2-MEMA-RIUB.jpg
[http2.0 5]: /pro/os/crawler/NN2A-M3AM-AAEA.jpg
[http2.0 6]: /pro/os/crawler/2MJE-7FAR-UBNE.jpg
[http2.0 7]: /pro/os/crawler/FJJI-ZAI3-QQEI.jpg
[http2.0 8]: /pro/os/crawler/NQQA-6JB6-R3IE.jpg
[http2.0 9]: /pro/os/crawler/MAJZ-IFBR-V2QF.jpg
[http2.0 10]: /pro/os/crawler/2EMZ-FVQQ-NF7J.jpg
[http2.0 11]: /pro/os/crawler/ANNE-ZZYE-J6RJ.jpg
[http2.0 12]: /pro/os/crawler/VU3Y-RIIB-BIV2.jpg
 *  **原文作者：** IT技术之家
 *  **原文链接：** https://www.toutiao.com/item/6563066005126382084/
 *  **版权声明：** ���博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。