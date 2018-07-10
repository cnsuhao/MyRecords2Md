---
title: 快速学会使用Fiddler抓包 截包伪造提交包
date: 2017-11-08 10:47:31
categories: "开发"
tags:
	- 脚本语言
	- 软件
	- 通信
	- HTML
	- Windows

---

**1、Fiddler介绍**

Fiddler是一个http协议调试代理工具，它能够记录并检查所有你的电脑,移动设备和互联网之间的http通讯，设置断点，查看所有的“进出”Fiddler的数据（指cookie,html,js,css等文件，这些都可以让你胡乱修改的意思）。 Fiddler 要比其他的网络调试器要更加简单，因为它不仅仅暴露http通讯还提供了一个用户友好的格式。它C\#写出来的,它包含一个简单却功能强大的基于JScript .NET 事件脚本子系统，它的灵活性非常棒，可以支持众多的http调试任务，并且能够使用.net框架语言进行扩展。官网也有很多插件提供,搜索引擎也能搜索到很多的相关插件，如有需求可自行搜索.它的强大之处就是可以自由的拦截到HTTP/HTTPS请求并解析内容,这个过程不仅仅是查看请求而已,这个工具是可以支持设断点修改请求后并再原请求发送或响应的。这也是很多Web调试工具做不到的。能查看请求流量的工具很多，但是能断点修改包并伪造的较少。

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_]

**工作原理**

**![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 1]**

Fiddler是以代理WEB服务器的形式工作的,浏览器与服务器之间通过建立TCP连接以HTTP协议进行通信，浏览器默认通过自己发送HTTP请求到服务器，它使用代理地址:127.0.0.1, 端口:8888. 当Fiddler开启会自动设置代理， 退出的时候它会自动注销代理，这样就不会影响别的程序。不过如果Fiddler非正常退出，这时候因为Fiddler没有自动注销，会造成网页无法访问。解决的办法是重新启动下Fiddler。

官方网站：https://www.telerik.com/fiddler 软件是支持免费使用的，原版是英文版，如有需要可自行搜索英文版。推荐使用英文版。

快速下载地址：https://telerik-fiddler.s3.amazonaws.com/fiddler/FiddlerSetup.exe 无须科学上网，国内即可正常访问下载，如地址失效请百度下载

**2、安装软件**

先下载好安装包，安装前先检查电脑是否已经安装了.NET Framework 4.0及其以上的版本，如果没有安装点我，一般现在的windows系统都是安装了的。环境准备好后开始安装:

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 2]

操作步骤:I Agree>Install>OK 这个工具有一个反人类的东西就是安装完成没有自动在桌面生成快捷键，所以还需要在安装的时候注意安装路径并找到目录去手动创建快捷键。

工具主页面：

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 3]

**3、配置软件**

1.打开fiddler配置Tools –> Fiddler Options.

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 4]

2.打开HTTPS配置项，勾选“CaptureHTTPS Connects”，同时勾选“Decrypt Https Traffic”，弹出的对话框选择是（这里是安装fiddler自己的证书）如果跟我一样手机跟电脑是用wifi进行链接的话还需要选择“…fromremote clients only”。如果需要监听不可信的证书的HTTPS请求的话，需要勾选“Ignore servercertificate errors”。

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 5]

3.打开Conections配置项， 这里可以修改Fiddler代理端口号。勾选“Allow remote computersto connect。提示需要重启fiddler。

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 6]

4.配置HTTPS请求解析,也是非常6B的一项功能之一

这里需要稍微写一点C\#的代码（复制就好）,找到工具的菜单栏 Ruler –>CustomizeRules 在函数OnBeforeResponse里面添加下面代码：

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 7]

    if (oSession.oRequest["User-Agent"].indexOf("Android") > -1 && oSession.HTTPMethodIs("CONNECT")) { oSession.oResponse.headers["Connection"] = "Keep-Alive";}

添加代码后的格式为:

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 8]

    static function OnBeforeResponse(oSession: Session) { if (m_Hide304s && oSession.responseCode == 304) { oSession["ui-hide"] = "true"; } if (oSession.oRequest["User-Agent"].indexOf("Android") > -1 && oSession.HTTPMethodIs("CONNECT")) { oSession.oResponse.headers["Connection"] = "Keep-Alive"; } }

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 8]

添加完成后记得Ctrl+S保存并重启Fiddler工具，这个时候就可以解析HTTPS流量并拦截了。

**4、移动设备进行抓包配置**

这里我拿iphone做举例,服务器填写的就是你的局域网电脑IP，端口就是上一步配置的端口，默认是8888.

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 9]

**4、进行实战截包断点修改**

**![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 10]**

以下是一个简单的例子:由于我的电脑使用了别的代理，所以一直提示"The system proxy was changed，click to reenable fiddler capture”

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 11]

 **5、使用小普及**

Fiddler的主界面分为 工具面板、会话面板、监控面板、状态面板

工具面板：

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 12]

说明注释、重新请求、删除会话、继续执行、流模式/缓冲模式、解码、保留会话、监控指定进程、寻找、保存会话、切图、计时、打开浏览器、清除IE缓存、编码/解码工具、弹出控制监控面板、MSDN、帮助

两种模式

 *  缓冲模式（Buffering Mode）Fiddler直到HTTP响应完成时才将数据返回给应用程序。可以控制响应，修改响应数据。但是时序图有时候会出现异常
 *  流模式（Streaming Mode）Fiddler会即时将HTTP响应的数据返回给应用程序。更接近真实浏览器的性能。时序图更准确,但是不能控制响应。

会话面板：

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 13]

控制面板：

![快速学会使用Fiddler抓包 截包伪造提交包][Fiddler_ 14]

**6、官网学习资料**

1.  Fiddler官方网站：http://www.telerik.com/fiddler
2.  Fiddler官方文档：http://docs.telerik.com/fiddler/configure-fiddler/tasks/configurefiddler
3.  Fiddler官方视频：https://www.youtube.com/playlist?list=PLvmaC-XMqeBbw72l2G7FG7CntDTErjbHc
4.  Fiddler官方插件：http://www.telerik.com/fiddler/add-ons


[Fiddler_]: /pro/os/crawler/2AE3-IZ3U-MARQ.jpg
[Fiddler_ 1]: /pro/os/crawler/UNRF-BNEA-BU2Y.jpg
[Fiddler_ 2]: /pro/os/crawler/RIQN-AYYV-6FII.jpg
[Fiddler_ 3]: /pro/os/crawler/7VVF-RNVA-7N3M.jpg
[Fiddler_ 4]: /pro/os/crawler/NZMU-IVYV-URBQ.jpg
[Fiddler_ 5]: /pro/os/crawler/IRJ6-ZZBU-N6VF.jpg
[Fiddler_ 6]: /pro/os/crawler/JEUM-EMIB-ENRY.jpg
[Fiddler_ 7]: /pro/os/crawler/IYVA-FEJE-YQIM.jpg
[Fiddler_ 8]: /pro/os/crawler/REAY-N2EN-7VBN.gif
[Fiddler_ 9]: /pro/os/crawler/QB3I-ANJF-UJ7Z.jpg
[Fiddler_ 10]: /pro/os/crawler/IZM7-BNB6-7NN2.jpg
[Fiddler_ 11]: /pro/os/crawler/V7JQ-IFQN-IIJU.gif
[Fiddler_ 12]: /pro/os/crawler/EUFR-YY2Q-RR7V.jpg
[Fiddler_ 13]: /pro/os/crawler/AFMQ-ZUJA-ZVFN.jpg
[Fiddler_ 14]: /pro/os/crawler/QVQV-J2RU-FVYE.jpg
 *  **原文作者：** David
 *  **原文链接：** https://www.toutiao.com/item/6485869846524330510/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。