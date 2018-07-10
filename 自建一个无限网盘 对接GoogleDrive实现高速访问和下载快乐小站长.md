---
title: 自建一个无限网盘 对接GoogleDrive实现高速访问和下载
date: 2018-06-19 17:51:13
categories: "开发"
tags:
	- 科技
	- Google
	- Linux
	- Wget

---

前言：目前国内网盘限速的限速，关闭的关闭，如何低成本拥有一个不限容量，不限速的网盘呢？

本文使用工具：服务器/VPS一个，GoogleDrive无限容量网盘一个（如何免费获取google无限网盘请关注我们，我们将在下篇文章中具体解说），域名一个（免费或者收费域名均可）

GoogleDrive是google推出的网盘服务，一般情况下能够获取15GB的免费空间，但是可以通过渠道获取无限容量。但是对于中国用户来说，唯一的使用方式就是挂载成服务器硬盘，然后自建一个网盘。

小贴士：

 *  服务器推荐 3TB流量/月，一个月18块人民币，无限网盘可以和朋友们共享平摊成本，基本可以满足需求了 点击这里购买：
 *  服务器不需要硬盘多大性能多好，只需要网络快，流量多，有渠道的也可以自行购买

> https://www.teching.ga/pq5v

域名最简单的解决方法是注册一个免费域名 注册地址：

> http://www.freenom.com/en/index.html?lang=en

言归正传，本次我们使用国内开发者开发的Cloudreve程序建立一个私有网盘（也可以开放注册功能成为一个公有网盘）

> Cloudreve：基于ThinkPHP构建的网盘系统，能够助您以较低成本快速搭建起公私兼备的网盘。
> 
> 目前已经实现的特性：
> 
> 快速对接多家云存储，支持七牛、又拍云、阿里云OSS、AWS S3、自建远程服务器，当然，还有本地存储
> 
> 可限制单文件最大大小、MIMEType、文件后缀、用户可用容量
> 
> 图片、音频、视频、文本、Markdown、Ofiice文档在线预览
> 
> 移动端全站响应式布局
> 
> 文件、目录分享系统，可创建私有分享或公开分享链接
> 
> 用户个人主页，可查看用户所有分享
> 
> 多用户系统、用户组支持
> 
> 初步完善的后台，方便管理
> 
> 拖拽上传、分片上传、断点续传、下载限速（\*实验性功能）
> 
> 多上传策略，可为不同用户组分配不同策略
> 
> 用户组基础权限设置、二步验证
> 
> WebDAV协议支持
> 
> 官方网站：https://cloudreve.org/
> 
> 程序下载：https://cloudreve.org/download.php
> 
> 项目地址：https://github.com/HFO4/Cloudreve/

首先安装LNMP环境，本文使用的是CentOS 7系统，关闭了firewalld, 安装了宝塔面板。

一步安装宝塔面板：

> yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh

按照提示一步步操作完以后，在宝塔面板中选择环境即可，非常方便。

然后新建一个网站和数据库，面板里都是中文的，能看得懂，本文就忽略不写了

要注意的是，创建完网站以后，需要在网站的伪静态配置中加入下面的代码：

> location / \{
> 
> if (!-e $request\_filename) \{
> 
> rewrite ^(.\*)$ /index.php?s=/$1 last;
> 
> break;
> 
> \}
> 
> \}

然后我们下载最新版的Cloudreve，并且上传到网站目录，并解压缩 下载地址：https://www.teching.ga/wwzg

在phpmyadmin中选中你刚才创建的数据库，导入下载包中的mysql.sql

在Cloudreve的目录中，找到

``````````
application/database_sample.php
``````````

将其修改为database.php, 选择编辑，填入你的数据库信息

现在你已经可以打开你自己的cloudreve了

> 默认的管理员账号：admin@cloudreve.org
> 
> 密码：admin
> 
> 后台地址：http://你的域名/Admin
> 
> 注：后台地址要先在前台用管理员账号登录后才能访问的到。

--------------------

接下来是如何对接GoogleDrive。目前，Cloudreve官方并不支持对接GoogleDrive和Onedrive，但是支持远程ftp，阿里OSS等，不过有路道的老板手里的GoogleDrive，对接以后也能物尽其用。

安装EPEL源：

``````````
yum -y install epel-release
``````````

安装一些基本组件和依赖：

``````````
yum -y install wget unzip screen fuse fuse-devel
``````````

下载Rclone解压然后进入目录：

``````````
wget https://downloads.rclone.org/rclone-v1.39-linux-amd64.zipunzip rclone-v1.41-linux-amd64.zipcd rclone-v1.41-linux-amd64
``````````

运行Rclone开始配置：

``````````
./rclone config
``````````

第一步选择n，然后回车输入一个name，建议这个name设置的简单好记一点，本例中我们创建一个名叫“gdrive”的网盘

然后选择我们要挂载的类型，这里选择10(google drive, 不是google cloud storage)，切记要选对了。

接着client\_id、client\_secret、service\_account\_file都留空直接回车，Use auto config?这里我们选择n，如图所示：

![自建一个无限网盘 对接GoogleDrive实现高速访问和下载][_GoogleDrive]

现在rclone会在终端内给我们回显一个GoogleDrive的授权登录地址，如图所示：

![自建一个无限网盘 对接GoogleDrive实现高速访问和下载][_GoogleDrive 1]

我们复制这个地址然后用本地电脑的浏览器打开并登录（需翻墙），然后点击允许按钮

接着复制如下图所示的授权代码：

![自建一个无限网盘 对接GoogleDrive实现高速访问和下载][_GoogleDrive 2]

回到终端内粘贴授权代码然后回车，继续按如下图操作，依次输入n、y、q：

完成以后用screen创建一个新的会话：

``````````
screen -S rclone
``````````

执行如下命令， 注意在宝塔面板下，我们的目录如下图所示，如果不是宝塔请自行修改

``````````
./rclone mount gdrive: /www/wwwroot/网站域名/public/uploads/gdrive --allow-other --allow-non-empty --vfs-cache-mode writes
``````````

不出意外的话，就挂载成功了

最后我们设置rclone开机自启：

``````````
cp /root/rclone-v1.41-linux-amd64/rclone /usr/bin/rclone
``````````

新建一个rclone.service文件：

``````````
vi /usr/lib/systemd/system/rclone.service
``````````

写入：

``````````
[Unit]Description=rclone [Service]User=rootExecStart=/usr/bin/rclone mount gdrive: /www/wwwroot/网站域名/public/uploads/gdrive --allow-other --allow-non-empty --vfs-cache-mode writesRestart=on-abort [Install]WantedBy=multi-user.target
``````````

重载daemon，让新的服务文件生效：

``````````
systemctl daemon-reload
``````````

现在就可以用systemctl来启动rclone了：

``````````
systemctl start rclone
``````````

设置开机启动：

``````````
systemctl enable rclone
``````````

停止、查看状态可以用：

``````````
systemctl stop rclonesystemctl status rclone
``````````

重启你的VPS，然后查看一下rclone的服务起来没，接着查看一下盘子挂上去没：

``````````
rebootsystemctl status rclonedf -h
``````````

--------------------

到这里，我们的配置已经全部完成，登陆Cloudreve的后台，新建一个上传策略，将上传目录设置为你googledrive挂载的目录，将用户的上传策略调整为你新建的上传策略，完工。


[_GoogleDrive]: /pro/os/crawler/AAUY-YBQR-ZYUQ.jpg
[_GoogleDrive 1]: /pro/os/crawler/3A2E-RFEU-NZBN.jpg
[_GoogleDrive 2]: /pro/os/crawler/ARZB-NVAR-2IEJ.jpg
 *  **原文作者：** 快乐小站长
 *  **原文链接：** https://www.toutiao.com/item/6568731027856949767/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。