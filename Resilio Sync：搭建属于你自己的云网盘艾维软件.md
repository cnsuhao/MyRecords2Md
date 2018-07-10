---
title: Resilio Sync：搭建属于你自己的云网盘
date: 2017-04-19 15:32:20
categories: "开发"
tags:
	- Linux
	- 软件
	- PuTTY
	- CentOS
	- Sync

---

Resilio Sync 是一款高效的数据传输工具，简单易用的多平台文件同步软件，惊人的传输速度是不同于其他产品的最大优势， Resilio Sync 的智能P2P技术加速同步，会将文件分割成若干份仅KB的数据同步，而文件都会进行 AES 加密处理。

随着国内众多网盘服务下线，留下来的受到多种限制，在这种情况下那么我们为什么不建立一个属于自己的网盘呢？即安全又放心，而且速度不会受任何因素影响，接下来小编就教大家如何在服务器上搭建一个属于自己的网盘：

# 准备工作： #

一台云服务器（比如阿里的ESC、腾讯的CVM）腾讯的CVM企业新用户可免费领取长达6个月的2核 4GB 5Mbps配置的服务器；

# **软件下载：** #

**http://www.resilio-sync.com/download/**

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync]

来自Resilio Sync官网  


> 本次是在腾讯CVM和阿里ECS上搭建，系统是linux CentOS 7.0 64位、CentOS 7+版本，因此请下载Liunx X64

# **安装服务：** #

第一步：打开PuTTY ，如果没有请下载：http://www.putty.org

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 1]

输入你的服务器IP地址，点击Open

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 2]

> 输入服务器登录用户名和密码（用户名默认为root）

第二步：进行安装Resilio Sync服务, 输入：

> wget https://download-cdn.resilio.com/stable/linux-x64/resilio-sync\_x64.tar.gz

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 3]

此时等待resilio-sync\_x64.tar.gz下载完成，完成后显示如下图：

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 4]

A. 解析文件， 输入：

> tar zxvf resilio-sync\_x64.tar.gz

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 5]

输入完成后，按下回车

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 6]

B. 查看帮助, 键入：./rslsync –help

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 7]

输入：  


> ./rslsync –dump-sample-config > sync.conf
> 
> vim sync.conf

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 8]

输入：i

PS: 对sync.conf文件进行编辑(按下两次回车，空出两行)

输入：

> “webui” :
> 
> \{
> 
> “listen” :”0.0.0.0:8888″ // remove field to disable WebUI
> 
> /\* preset credentials. Use password or password\_hash \*/
> 
> // ,”login” : “admin”
> 
> // ,”password” :”password”
> 
> // ,”password\_hash” : “some\_hash” // password hash in crypt(3) format
> 
> // ,”allow\_empty\_password” : false // deraults to true
> 
> /\* ssl configuration \*/
> 
> // ,force\_https” :true // disable http
> 
> // ,ssl\_certificate” : “/path/to/cert.pem”
> 
> // ,ssl\_private\_sky” : “/path/to/private.key”
> 
> /\* dir\_whitelist defines which directories can be shown to user or have folders added (linux only) relative paths are relative to directory\_root setting \*/
> 
> // ,”dir\_whitelist” :\[ “/home/user/MysharedFolders/personal”, “work”\]
> 
> \}

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 9]

按下：Esc ，键入：【：wq】按下回车

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 10]

键入：./rslsync –webui.listen 0.0.0.0:8888

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 11]

当你看到页面显示如上图时，说明你已经安装好了Resilio Sync,接下来我们需要对Resilio Sync进行设置

# **配置Resilio Sync：** #

在你的浏览器中输入：IP：8888 (IP是您服务器的地址)

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 12]

初始化创建您的管理账户（帐户和两次一样的密码，再点击Continue）

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 13]

输入一个名称来显示，当您发送和接收文件夹时候名称（可默认为root）,然后勾选下面两项协议 再点击Get started

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 14]

使用你刚刚设置的帐户密码进行登录

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 15]

输入您的电子邮箱地址

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 16]

然后进入操作主页面（在这里你可以看到和客户端显示的基本上是一样的）

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 17]

接下来，需要创建一个共享文件夹

点击左上角：Add Folder

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 18]

创建好文件夹，然后点击右侧的Share 共享将密钥发到你的客户端进行连接

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 19]

# **安装客户端：** #

下载：http://www.resilio-sync.com/download/

请下载对应的操作系统版本

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 20]

安装好客户端，然后打开软件

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 21]

点击左上角下拉菜单，选择输入密钥或链接选项

![Resilio Sync：搭建属于你自己的云网盘][Resilio Sync 22]

输入你从服务器复制来的密钥点击下一步进行连接，然后在本地设置一个存放文件的文件夹

此时你已经完成了所有设置，你可以在你的所有设备中进行同步，你还可以创建一个共享的文件夹与同事和朋友协作

# **激活Resilio Sync：** #

安装好Resilio Sync服务之后你可以免费使用30天全功能

购买**Resilio Sync许可证：http://www.resilio-sync.com/store/**

个人用户购买专业版，团队和工作组需要购买Workgroups，功能差异课请阅读相关介绍

原文地址：https://www.resilio-sync.com/blog/


[Resilio Sync]: /pro/os/crawler/VUUI-NNQ7-7REY.jpg
[Resilio Sync 1]: /pro/os/crawler/6NFI-ERJJ-ZNNN.jpg
[Resilio Sync 2]: /pro/os/crawler/EEQE-YVMM-I2AZ.jpg
[Resilio Sync 3]: /pro/os/crawler/MRAE-MFQM-IRIA.jpg
[Resilio Sync 4]: /pro/os/crawler/NNBQ-IIAM-JURR.jpg
[Resilio Sync 5]: /pro/os/crawler/EJYA-M3BQ-FF2Q.jpg
[Resilio Sync 6]: /pro/os/crawler/NMUU-IAMN-JIJ2.jpg
[Resilio Sync 7]: /pro/os/crawler/2YU6-ZYYQ-7RNE.jpg
[Resilio Sync 8]: /pro/os/crawler/VUFI-YNUJ-M6JN.jpg
[Resilio Sync 9]: /pro/os/crawler/2YF7-J3UE-I3MI.jpg
[Resilio Sync 10]: /pro/os/crawler/FYAZ-AVRV-6BUJ.jpg
[Resilio Sync 11]: /pro/os/crawler/BNY6-FIYI-QJUB.jpg
[Resilio Sync 12]: /pro/os/crawler/RMBN-7NYJ-MBNF.jpg
[Resilio Sync 13]: /pro/os/crawler/JBZQ-I3JJ-BJFN.jpg
[Resilio Sync 14]: /pro/os/crawler/YVY6-FFFE-JVAR.jpg
[Resilio Sync 15]: /pro/os/crawler/QJ2Y-ZBNI-6ZIM.jpg
[Resilio Sync 16]: /pro/os/crawler/EZJQ-7BAQ-NBJ2.jpg
[Resilio Sync 17]: /pro/os/crawler/AQQJ-JF7B-IFNU.jpg
[Resilio Sync 18]: /pro/os/crawler/Y6VV-I2NI-Y7N3.jpg
[Resilio Sync 19]: /pro/os/crawler/YARB-YM3I-VQZU.jpg
[Resilio Sync 20]: /pro/os/crawler/ZIEN-QVNN-VNYV.jpg
[Resilio Sync 21]: /pro/os/crawler/MJZV-I3AZ-FUZN.jpg
[Resilio Sync 22]: /pro/os/crawler/NRRF-FE2U-BEMQ.jpg
 *  **原文作者：** 艾维软件
 *  **原文链接：** https://www.toutiao.com/item/6410612914876580354/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。