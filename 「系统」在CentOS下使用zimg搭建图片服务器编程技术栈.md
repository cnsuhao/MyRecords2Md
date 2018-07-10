---
title: 「系统」在CentOS下使用zimg搭建图片服务器
date: 2017-11-02 09:54:32
categories: "开发"
tags:
	- Java
	- Linux
	- GitHub
	- 软件
	- CentOS

---

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg]

zimg 图片服务器

# 介绍 #

> zimg是一种轻量级高性能图像存储与处理系统。  
> 

官网地址：http://zimg.buaa.us/

GitHub：https://github.com/buaazp/zimg

# 环境搭建 #

官方安装说明地址：http://zimg.buaa.us/documents/install/

此次搭建的操作系统为CentOS Linux 7（如何在Windows下使用虚拟器安装Linux操作系统？请参考文章《[Windows10下安装VMware虚拟机并搭建CentOS系统环境][Windows10_VMware_CentOS]》），根据官方文档要求，在安装zing之前需要配置环境，运行以下命令进行安装软件依赖。

> sudo yum install openssl-devel cmake libevent-devel libjpeg-devel giflib-devel libpng-devel libwebp-devel ImageMagick-devel libmemcached-devel

每个安装包具体说明在此暂不做介绍，你可以通过运行以下命令来查看各个安装包是否已经安装，并查看其版本。：  


> rpm -qa | grep openssl

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 1]

查看程序安装信息

以上软件安装完毕后，接下来就可以安装zimg服务了，安装方法有两种，第一种是clone（克隆）GitHub源码到本地，并make（编译），但是我试了几次都没有成功，总是出现以下错误信息：

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 2]

make zimg 错误

如果有哪位朋友能运行成功，希望能告知其配置环境或解决办法；另外一种方法就是直接下载安装包文件（软件版本为v3.1.0），文件下载地址：

\[官方\] https://github.com/buaazp/zimg/releases/download/v3.1.0/zimg-3.1.0-Release.x86\_64.rpm

\[百度\] https://pan.baidu.com/s/1eRYm88M

下载好文件后，运行以下命令进行安装：

> sudo rpm -ivh zimg-3.1.0-Release.x86\_64.rpm \#运行安装

# 服务运行和停止 #

> /usr/local/zimg/zimg /usr/local/zimg/conf/zimg.lua \#运行

执行上面的执行便可以启动zimg服务，由上面的命令可以看出，zimg的配置文件为/usr/local/zimg/conf/zimg.lua，配置文件默认以后台模式运行，运行结果如下图所示：

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 3]

运行zimg服务

服务开启后通过浏览器访问 http://127.0.0.1:4869 查看检查，查下如下页面即服务器启动完成。

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 4]

zimg启动浏览器访问页面

如要外网无法访问，可尝试更改防火墙配置开放4869端口，运行如下命令（CentOS 7）：

> firewall-cmd --zone=public --add-port=4869/tcp --permanent

\# 其中命令中的参数 --permanent 为永久生效，没有此参数重启后失效。

注：还有一种方法是安装“iptables-services”，更改其配置文件。

> yum -y install iptables-services \# 安装iptables-services服务
> 
> \#如果要修改防火墙配置，如增加防火墙端口4869
> 
> vi /etc/sysconfig/iptables
> 
> \# 增加规则
> 
> \-A INPUT -m state --state NEW -m tcp -p tcp --dport 4869 -j ACCEPT

zimg服务启动后，可通过以下命令查看运行进程ID（程序默认使用端口为4869）

> netstat -lnp | grep 4869

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 5]

zing 运行进程

使用以下命令进行关闭zimg服务，暂未找到zimg服务（后台运行的）正常关闭的命令，只能强制杀死进程。

> kill -9 31618

# 使用 #

启动zimg服务，在浏览器中打开网页 http://127.0.0.1:4869，点击“选择文件”按钮选择一张图片，然后再点击“Upload”按钮进行上传，上传成功后出现以下界面：

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 6]

图片访问链接

可点击前面的URL进行访问，也可以根据后面的参数，进行组合使用。

部分参数说明：

> w：图片宽度
> 
> h：图片高度
> 
> g：是否是黑白图片（1：是）
> 
> q：压缩比
> 
> f：转换格

上传的图片文件存放在 /usr/local/zimg/img （zimg服务安装目录下的img目录中），根据不同的参数访问会生成不图的文件，大家可以自行试验查看。

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 7]

根据访问生成不同尺寸或样式的文件

# 管理 #

启动服务，在浏览器中打开网页 http://127.0.0.1:4869/admin（如下图），进入管理页面，可以在该页面进行对图片的删除操作。输入图片的MD5值，点击“execute”进行执行删除操作，将会删除该图片所有格式的图片。

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 8]

管理图片（删除图片）

出现一下界面说明执行操作成功。

![「系统」在CentOS下使用zimg搭建图片服务器][CentOS_zimg 9]

执行操作成功

关于zimg服务的更多配置和使用说明，可访问官网服务指南，（中文）地址为：http://zimg.buaa.us/documents/guidebookcn/ 。

接下面会发布一篇有关zimg服务API的使用，整合Spring（JAVA）进行开发介绍，敬请关注。


[CentOS_zimg]: /pro/os/crawler/MBJ3-M3UN-NBMV.jpg
[Windows10_VMware_CentOS]: https://www.toutiao.com/i6482968499277791757/
[CentOS_zimg 1]: /pro/os/crawler/RNFR-JUJ7-3ERU.jpg
[CentOS_zimg 2]: /pro/os/crawler/JFEJ-UBRQ-EYNF.jpg
[CentOS_zimg 3]: /pro/os/crawler/YAJF-IVZQ-UMZ3.jpg
[CentOS_zimg 4]: /pro/os/crawler/NEBR-EAAU-ZEY3.jpg
[CentOS_zimg 5]: /pro/os/crawler/ENQ2-EVMI-MIVB.jpg
[CentOS_zimg 6]: /pro/os/crawler/YNNN-3IEF-VJIB.jpg
[CentOS_zimg 7]: /pro/os/crawler/BFVB-ABEZ-BF6Z.jpg
[CentOS_zimg 8]: /pro/os/crawler/QMJ2-EEBJ-MJR3.jpg
[CentOS_zimg 9]: /pro/os/crawler/RNZM-7RMZ-MFNJ.jpg
 *  **原文作者：** 编程技术栈
 *  **原文链接：** https://www.toutiao.com/item/6483297774073807374/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。