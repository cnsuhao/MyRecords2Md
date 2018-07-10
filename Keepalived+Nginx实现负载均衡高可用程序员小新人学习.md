---
title: Keepalived+Nginx实现负载均衡高可用
date: 2018-06-23 11:01:23
categories: "开发"
tags:
	- Nginx
	- 技术

---

**一、负载均衡高可用**

Nginx作为负载均衡器，所有请求都到了Nginx，可见Nginx处于非常重点的位置，如果Nginx服务器宕机后端web服务将无法提供服务，影响严重。

为了避免负载均衡服务器的宕机故障，需要建立一个备份机。主备机上都运行高可用（High Availability）监控程序，通过传送心跳信息来监控对方的运行状况。当备份机不能在一定的时间内收到对方的正常心跳时，它就接管主服务器的服务IP并继续提供负载均衡服务；当备份管理器又从主管理器收到“I am alive”这样的信息时，它就释放服务IP地址，这样的主服务器就开始再次提供负载均衡服务。

**二、使用keepalived+Nginx实现负载均衡高可用**

1、提供两个Nginx负载服务器

这里方便演示，分别在本机上添加2个虚拟服务器，分别安装Nginx

2、分别在两台服务器上安装keepalived

Keepalived的安装方式不外乎检查配置、编译、安装那几个命令，这里就不再赘述，为方便管理，将相关配置文件进行移动，重启keepalived服务

![Keepalived+Nginx实现负载均衡高可用][Keepalived_Nginx]

3、配置keepalived

安装好keepalived后 ，进入/usr/local/keepalived/etc/keepalived，修改keepalived.conf文件

1）主机

![Keepalived+Nginx实现负载均衡高可用][Keepalived_Nginx 1]

2）备机

![Keepalived+Nginx实现负载均衡高可用][Keepalived_Nginx 2]

通过对两台服务器的keepalive进行配置，区分出主机和备机服务器，state MASTER 为主机，priority 优先级值大于备机，state BACKUP为备机。

配置好keepalived之后，分别启动两台服务器上的nginx和keepalived进行测试。

4、测试

1）查看主机的nginx，发现keepalived的虚拟IP绑定在主服务器上nginx上，

![Keepalived+Nginx实现负载均衡高可用][Keepalived_Nginx 3]

而备份服务器却提示not exsit

![Keepalived+Nginx实现负载均衡高可用][Keepalived_Nginx 4]

这就说明服务一启动，keepalived的虚拟IP绑定在主服务器的eth0网卡上.另外将主服务器的nginx关闭后，再查看，发现keepalived的vip立刻绑定在了备服务器的eth0上，当主服务器恢复工作时，VIP又自动切换回来。这样就实现了通过keepalived这个工具来监测多台服务器的工作状态，当主服务器宕机后，可智能切换到可用备机，从而避免了单点故障问题。


[Keepalived_Nginx]: /pro/os/crawler/YAIN-AYZV-BQV3.jpg
[Keepalived_Nginx 1]: /pro/os/crawler/2UEB-FQYR-QVQA.jpg
[Keepalived_Nginx 2]: /pro/os/crawler/ZJNZ-I2N6-NQIQ.jpg
[Keepalived_Nginx 3]: /pro/os/crawler/YZNZ-BFZN-EJUU.jpg
[Keepalived_Nginx 4]: /pro/os/crawler/6VJB-AIF2-2EUB.jpg
 *  **原文作者：** 程序员小新人学习
 *  **原文链接：** https://www.toutiao.com/item/6570109756587901447/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。