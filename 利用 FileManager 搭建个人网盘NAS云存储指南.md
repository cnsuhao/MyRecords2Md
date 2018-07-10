---
title: 利用 FileManager 搭建个人网盘
date: 2018-04-12 17:00:47
categories: "开发"
tags:
	- 文本编辑器
	- 路由器
	- Go语言
	- Vim
	- Windows

---

![利用 FileManager 搭建个人网盘][FileManager]

为什么需要自己搭建网盘

 *  目前网盘大部分不是限速就是限制容量，体验太差；
 *  分享不受限制，高级功能不受会员限制；
 *  具有局域网最大速度访问优势，在线浏览图片、影音速度都可以慢速需求。

FileManager 是什么？

 *  基于 Go 语言提供 Web 交互的文件管理程序，操作方便，轻量级搭建极其简单，多平台支持，可以运行在路由器中；
 *  多用户管理，可以共享系统给他人使用；
 *  文件上传、下载(支持打包下载)、预览、文档在线编辑功能、目录和文件管理创建。

我的使用场景

 *  临时分享文件使用。如果搭建在服务器上，服务器空间昂贵，但是上传和下载速度比普通宽���快，分享临时文件方便快捷；
 *  搭建在路由上，然后路由器通过 frp 内网穿透，远程访问家中路由器上文件，相对 NAS 更加省电，浏览图片和音乐家里的上传带宽也够用；
 *  相对于 Smb、ftp、webdav 连接过程更简单，用于在公用电脑上下载自己常用的软件，不需要安装任何客户端软件。直接建立一个公共只读账户，放一些自己常用的工具软件，方便传输。

如何搭建？

``````````
curl -fsSL https://henriquedias.com/filemanager/get.sh | bash
``````````

或者

``````````
wget -qO- https://henriquedias.com/filemanager/get.sh | bash
``````````

Windows 平台使用 PowerShell

``````````
iwr -useb https://henriquedias.com/filemanager/get.ps1 | iex
``````````

配置 FileManager

**简单配置**

 *  port：服务所用端口
 *  database：数据库存放地址
 *  scope：需要管理的文件夹路径
 *  allowCommands：允许使用指令
 *  allowEdit：允许进����件编辑操作
 *  allowNew：允许进行新建文件和文件夹操作
 *  更多配置请参考官方文档 \[配置文档\](https://henriquedias.com/filemanager/configuration/)

``````````
vim /etc/file/config.json# 文件内容{ "port": 1250, "database": "/etc/database.db", "scope": "/home/zhangyongcun/doc/", "allowCommands": true, "allowEdit": true, "allowNew": true}
``````````

运行服务

``````````
filemanager -c /etc/file/config.json
``````````

加入 supervisor

``````````
vim /etc/supervisor/conf.d/file.conf# 文件内容[program:file]user=rootcommand=filemanager -c /etc/file/config.jsonprocess_name=%(program_name)sautostart=trueredirect_stderr=truestdout_logfile=/var/log/file.logstdout_logfile_maxbytes=1MBstdout_logfile_backups=0
``````````

使用 nginx 转发端口

``````````
vim /etc/nginx/sites-enabled/file# 文件内容server { listen 80; server_name file.xxxx.com; location / { proxy_pass http://localhost:1250; }}
``````````

访问：file.xxxx.com 或者 xxxx.com:1250 即可，默认账户:admin 默认密码：admin 登陆之后可以进行密码重置。

界面预览

基本功能

![利用 FileManager 搭建个人网盘][FileManager 1]

多用户管理

![利用 FileManager 搭建个人网盘][FileManager 2]

更多高级功能，请参考 FileManager


[FileManager]: http://p9.pstatp.com/large/pgc-image/152352362332886b21b7743
[FileManager 1]: http://p3.pstatp.com/large/pgc-image/1523523623617f2249ff832
[FileManager 2]: http://p3.pstatp.com/large/pgc-image/152352362330043d534072b
 *  **原文作者：** NAS云存储指南
 *  **原文链接：** https://www.toutiao.com/item/6543484239516533262/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。