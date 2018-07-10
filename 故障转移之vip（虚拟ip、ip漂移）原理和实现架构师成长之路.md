---
title: 故障转移之vip（虚拟ip、ip漂移）原理和实现
date: 2018-05-22 17:55:08
categories: "开发"
tags:
	- 技术
	- 笔记本电脑
	- Mac

---

在系统架构设计的时候，我们经常听说vip、ip漂移，其实本质来说就是虚IP，也叫vip，我们今天就讲一下什么是vip。

# 原理篇  #

其实现原理主要是靠TCP/IP的ARP协议。因为ip地址只是一个逻辑 地址，在以太网中MAC地址才是真正用来进行数据传输的物理地址，每台主机中都有一个ARP高速缓存，存储同一个网络内的IP地址与MAC地址的对应关 系，以太网中的主机发送数据时会先从这个缓存中查询目标IP对应的MAC地址，会向这个MAC地址发送数据。操作系统会自动维护这个缓存。这就是整个实现 的关键。

![故障转移之vip（虚拟ip、ip漂移）原理和实现][vip_ip_ip]

我的arp缓存

# 实践篇 #

ifconfig eth0:1 192.168.109.108 netmask 255.255.255.0 //增加虚拟网卡，并且设置ip、网关等

ifconfig eht0:1 up //启动网卡

这样我们就能ping通这个ip了

ping -c 3 192.168.109.108

\# ping -c 3 192.168.109.108

PING 192.168.109.108 (192.168.109.108) 56(84) bytes of data.

64 bytes from 192.168.109.108: icmp\_seq=1 ttl=64 time=0.032 ms

64 bytes from 192.168.109.108: icmp\_seq=2 ttl=64 time=0.053 ms

64 bytes from 192.168.109.108: icmp\_seq=3 ttl=64 time=0.036 ms

# 故障转移篇  #

![故障转移之vip（虚拟ip、ip漂移）原理和实现][vip_ip_ip 1]

上图就详细描述了故障转移过程，比较简单，不再详述


[vip_ip_ip]: /pro/os/crawler/YZF3-MFUU-ZN2Q.jpg
[vip_ip_ip 1]: /pro/os/crawler/6F6F-6ZR3-Y2IQ.jpg
 *  **原文作者：** 架构师成长之路
 *  **原文链接：** https://www.toutiao.com/item/6558341653722038787/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。