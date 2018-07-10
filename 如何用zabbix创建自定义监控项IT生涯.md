---
title: 如何用zabbix创建自定义监控项
date: 2018-01-31 08:59:09
categories: "开发"
tags:
	- 脚本语言
	- 技术
	- AWK
	- MySQL
	- CentOS

---

**背景：**

zabbix本身提供了很多可选的监控项，可以满足绝大部分的监控需求。有时候由于业务需求，需要自定义监控项。 下面以创建mysql自定义监控项为例，分享如何创建zabbix自定义监控项。

**环境说明：**

zabbix版本：3.0.3 操作系统：CentOS 7 mysql版本：5.7.1

**实现步骤：**

1、修改 zabbix\_agentd.conf，添加zabbix\_agent 配置目录，以下是我本机的zabbix的配置： 将以下行的注释去掉

``````````
#Include=/usr/local/etc/zabbix_agentd.conf.d/*.conf
``````````

变成：

``````````
Include=/usr/local/etc/zabbix_agentd.conf.d/*.conf
``````````

将此行注释去掉后，zabbix\_agentd启动后会自动扫描/usr/local/etc/zabbix\_agentd.conf.d/目录下所有的.conf文件，并加载。

2、编写监控脚本/usr/local/zabbix/zabbix-script/get\_mysql\_status.sh，脚本如下(脚本存放目录可以自定义)：

``````````
#!/bin/shcase $3 inuptime)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $2}';;threads)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $4}';;question)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $6}';;sq)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $9}';;open)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $11}';;ftable)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $14}';;opent)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $17}';;qps)mysqladmin -u$1 -p$2 status 2>/dev/nul | awk -F '[:| ]'+ '{print $22}';;*)
``````````

脚本说明，脚本需要输入三个参数分别是：mysql用户、mysql用户密码、mysql状态各项指标如下： uptime:运行时长单位s、 threads：开启的会话数、 question（questions）：服务器启动以来客户的问题(查询)数目 sq（Slow queries）: 慢查询数量 open（opens）：服务器已经打开的数据库表的数量 ftable(Flush tables):服务器已经执行的flush ...、refresh和reload命令的数量 opent(open tables):通过命令是用的数据库的表的数量，以服务器启动开始 qps（Queries per second avg）：select语句平均查询时间

3、在/usr/local/etc/zabbix\_agentd.conf.d/目录下添加监控项配置文件get\_mysql\_status.conf，内容如下：

``````````
UserParameter=get_mysql_status[*],/usr/local/zabbix/zabbix-script/get_mysql_status.sh $1 $2 $3
``````````

4、重启zabbix\_agent和zabbix\_server，使用zabbix\_get测试，如下：

``````````
#zabbix_get -s 127.0.0.1 -k get_mysql_status[root,weiming,open]679
``````````

5、web端添加监控项： 在主机上添加监控项：

![如何用zabbix创建自定义监控项][zabbix]

添加完成后可以看到新增监控项如下：

![如何用zabbix创建自定义监控项][zabbix 1]

添加图形：

![如何用zabbix创建自定义监控项][zabbix 2]

图形预览：

![如何用zabbix创建自定义监控项][zabbix 3]


[zabbix]: /pro/os/crawler/Z3EE-EFE2-6R6B.jpg
[zabbix 1]: /pro/os/crawler/YZN6-BI7J-RUVN.jpg
[zabbix 2]: /pro/os/crawler/FZAZ-UUAY-Z2II.jpg
[zabbix 3]: /pro/os/crawler/QNVI-RMQE-ZZJ3.jpg
 *  **原文作者：** IT生涯
 *  **原文链接：** https://www.toutiao.com/item/6517013075571245575/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。