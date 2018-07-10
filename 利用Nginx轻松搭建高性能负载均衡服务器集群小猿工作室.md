---
title: 利用Nginx轻松搭建高性能负载均衡服务器集群
date: 2018-05-26 12:54:35
categories: "开发"
tags:
	- Nginx
	- Tomcat
	- Apache

---

在开始之前不得不提一下Nginx这个俄罗斯战斗民族开发的web服务器实在强大。特别是作为静态网站服务器简直是合适到没谱。废话不多说，，开始操练。

一、工具

1.  Nginx
2.  apache-tomcat

二、效果图

![利用Nginx轻松搭建高性能负载均衡服务器集群][Nginx]

三、实现步骤

1、首先下载Nginx，要下载稳定版

2、下载tomcat，可以下载免安装版，然后复制一遍。分别开启两个tomcat查看是否可用

3、修改其中一个tomcat的端口号（默认是8080，可以修改其中一个为8081），修改完成后确认是否可用（不会的可以私信我或者自行百度）

4、将其中一个tomcat的index页面稍作修改已做区分，并检查是否可以正常访问

5、修改Nginx配置文件

![利用Nginx轻松搭建高性能负载均衡服务器集群][Nginx 1]

核心配置，可以配置weight数值分配服务器访问的权重分配。

![利用Nginx轻松搭建高性能负载均衡服务器集群][Nginx 2]

四、分别开启两个tomcat，重启nginx，浏览器访问localhost/index.jsp。

我的配置是8.5.16这个的权重是3，

第一次访问的是Apache tomcat8.5.16，刷新浏览器

![利用Nginx轻松搭建高性能负载均衡服务器集群][Nginx 3]

第二次访问：依旧是Apache tomcat 8.5.16这个服务器，继续刷新依旧是Apache tomcat 8.5.16,出错了吗？？？

![利用Nginx轻松搭建高性能负载均衡服务器集群][Nginx 4]

第四次访问：本次访问的8.0.21。接下来继续不断刷新，结果基本是访问3次8.5.16后访问1次8.2.21。

![利用Nginx轻松搭建高性能负载均衡服务器集群][Nginx 5]

OK ,,配置成功。此外由于Nginx的强大特性，我们还 可以将静态资源单独配置，加快网页加载速度。

我们是小猿工作室，一群执着于技术的年轻人，欢迎收藏转发哦，喜欢我们的分享就点击关注我们吧。


[Nginx]: /pro/os/crawler/UVRR-Z3YA-IFZ2.jpg
[Nginx 1]: /pro/os/crawler/MZZJ-RNRF-ZEZI.jpg
[Nginx 2]: /pro/os/crawler/ZIQ6-FBBZ-EMEF.jpg
[Nginx 3]: /pro/os/crawler/UJQV-FJJB-BFBQ.jpg
[Nginx 4]: /pro/os/crawler/JEQB-2UNA-AYBN.jpg
[Nginx 5]: /pro/os/crawler/MUNI-2YAY-NNAF.jpg
 *  **原文作者：** 小猿工作室
 *  **原文链接：** https://www.toutiao.com/item/6556900523792925197/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。