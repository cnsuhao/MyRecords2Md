---
title: 9、Nginx HTTP负载均衡和反向代理简述
date: 2018-02-02 11:31:40
categories: "开发"
tags:
	- Nginx
	- 思科系统
	- Linux
	- 软件
	- DNS

---


![9、Nginx HTTP负载均衡和反向代理简述][9_Nginx HTTP]

在说Nginx HTTP负载均衡和反向代理之前，我 们先来说说什么负载均衡和反向代理。


负载均衡

负载均衡是有多台服务器以对称的方式组成一个服务器集合，每台服务器都具有等价的地位，都可以单独对外提供服务而无须其他服务器的辅助。通过某种负载分担技术，将外部发来的请求均匀分配到对称结构中的某一台服务器上，而接受到请求的服务器单独地回应客户的请求。均衡负载能够平均分配客户请求到服务器阵列，以此快速获取重要数据，解决大量并发访问服务问题。这种集群技术可以用最少的投资获得接近于大型主机的性能

反向代理

反向代理（Reverse Proxy）是指以代理服务器来接受Internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上的到的结果返回给Internet上请求连接的客户端，此时代理服务器对外就表现为一个服务器。

常见的Web负载均衡方法。主要分为以下几种常见的负载均衡方式：

**用户手动选择方式**。这是一种较为古老的方式，通过主页面首页入口提供不同线路、不同服务器链接的方式，来实现负载均衡，例如：新浪首页

![9、Nginx HTTP负载均衡和反向代理简述][9_Nginx HTTP 1]

**DNS轮训方式**。大多域名注册商都支持对同一主机名添加多条A记录，这就是DNS轮训，DNS服务器将解析请求按照A记录的顺序，随机分配到不同的IP上，这样就完成了简单的负载均衡。但是确定也比较明显：可靠性低、由于客户本地计算机缓存导致负载均衡不均衡。

***四/七层负载均衡设备***

硬件四/七层负载均衡交换机代表有：F5 BIG-IP、Citrix NetSaler、Radware 、Cisco CSS、Foundry等产品。下图是一张F5 BIG-IP实现动、静态网页分离的负载均衡架构图。

假设域名blog.s135.com被解析到F5的外网/公网虚拟ip：61.1.13（vs\_squid），该虚拟IP下有一个服务器池（pool\_squid），该服务器池下包含俩台真是的Squid服务器（192.168.1.11和192.168.1.12）

![9、Nginx HTTP负载均衡和反向代理简述][9_Nginx HTTP 2]

![9、Nginx HTTP负载均衡和反向代理简述][9_Nginx HTTP 3]

软件四层负载均衡的代表作品为LVS(Linux Virtual Server)，其采用IP负载均衡技术和基于内容请求分发技术。

![9、Nginx HTTP负载均衡和反向代理简述][9_Nginx HTTP 4]

![9、Nginx HTTP负载均衡和反向代理简述][9_Nginx HTTP 5]

软件的七层负载均衡大多基于HTTP反向代理方式，代表产品有Nginx、L7sw（Layer7 switching）、HAProxy等。Nginx的反向代理负载均衡能够很好地支持虚拟主机，可配置性很强，可以按轮询、IP哈希、URL哈希、权重等多种方式对后端服务器做负载均衡，同时还支持后端服务器的健康检查。

**大家觉得写得好的话，请关注我哦，下一节着重讲讲《Nginx反向代理实例》**


[9_Nginx HTTP]: /pro/os/crawler/26ZA-BFQV-NJFQ.jpg
[9_Nginx HTTP 1]: /pro/os/crawler/QY6R-VNZ3-UIAJ.jpg
[9_Nginx HTTP 2]: /pro/os/crawler/Q6B6-RYEY-VJMU.jpg
[9_Nginx HTTP 3]: /pro/os/crawler/A6FV-AEVZ-ZAQF.jpg
[9_Nginx HTTP 4]: /pro/os/crawler/6VVI-N27N-RARN.jpg
[9_Nginx HTTP 5]: /pro/os/crawler/VQUU-YUZM-3EU2.jpg
 *  **原文作者：** IT那达慕
 *  **原文链接：** https://www.toutiao.com/item/6517794549002142222/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。