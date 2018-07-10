---
title: testlink测试用例系统搭建完善文档，一个不错的测试管理工具
date: 2018-02-05 11:13:24
categories: "开发"
tags:
	- Nginx
	- 技术
	- MySQL
	- 编程语言
	- PHP

---

# **1.为什么要搭建testlink系统**  #

testlink是基于web的测试用例管理系统，主要功能是:测试项目管理、产品需求管理、测试用例管理、测试计划管理、测试用例的创建、管理和执行，并且还提供了统计功能。

我们都知道，做技术产品项目开发，测试是很重要的一环，如果没有有效的测试管理手段，那么做出来的产品到底有没有bug，有没有达到效果，那就难说了。


![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink]

testlink登录界面

# **2.官方下载testlink** #

wget http://nchc.dl.sourceforge.net/project/testlink/TestLink%201.9/TestLink%201.9.13/testlink-1.9.13.tar.gz

1.9.13版本并不是最新的版本，大家想用什么版本可以根据自己的要求选择，其大体的安装方式基本无异。

# **3.环境准备** #

我的服务器上面原先已经有了zabbix的LNMP环境，因此在这里就不需要再搭建多一个环境了，直接利用这个环境进行工作。

需要nginx、php、mysql等，需要自己进行安装，如果不会安装，可以在评论中回复，我稍后可以再出一个一键安装nginx，php，mysql的方法。

# **4.配置权限属性** #

**1）、配置testlink权限属性**

\# tar xf testlink-1.9.13.tar.gz

\# mv /home/ihavecar/testlink/testlink-1.9.13 /usr/local/wwwweb/

\# mv testlink-1.9.13 testlink

\# chown nobody.nobody testlink/ -R

\# chmod 755 testlink/ -R

\# chmod 777 testlink/gui/templates\_c -R

**2）、配置nginx代理**

在主配置文件中添加：include testlink.conf;

\# cat testlink.conf

server \{

listen 15111;

location / \{

root /usr/local/wwwweb/testlink;

index index.php;

\}

location ~ .(php|phtml)$ \{

root /usr/local/wwwweb/testlink;

fastcgi\_pass 127.0.0.1:9000; \#php端口，类似于nginx的proxy

fastcgi\_index index.php;

fastcgi\_param SCRIPT\_FILENAME $document\_root$fastcgi\_script\_name;

include fastcgi\_params;

\}

\}

重新reload nginx即可生效。

# **5.安装testlink** #

**1)、浏览器访问**

http://192.168.1.15:15111

点击 New installation

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 1]

开始讲协议，你agree就行了，第一个步骤就是同意协议即可。

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 2]

**2)、检查操作系统以及所需要求是否符合，有一些报错，那就满足她**

\# mkdir /var/testlink/logs/ -p

\# mkdir /var/testlink/upload\_area/ -p

\# chown nobody.nobody testlink/ -R 除了按她要求帮她创建两个目录，你还要给她权限，要不然她都不鸟你

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 3]

满足了她之后，她终于乖乖地从了。

点击 continue

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 4]

**3)、定义数据库接入配置**

页面的最后会告诉你，如果你都已经是成功通过数据库定义的话，那么你就可以通过账号admin，密码admin作为管理员去登录这个testlink系统。

After successfull installation You will have the following login for TestLink Administrator:

login name: admin

password : admin

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 5]

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 6]

**4)、开始按照你的数据库定义设置去创建库啊，用户等信息**

过程中又开始抽风了，说我给他的root@192.168.1.15 这个用户没有创建用户的权限，因此需要通过root@localhost登录进去mysql直接手动帮她创建吧，免得她着急：

mysql> grant all on testlink.\* to 'testlinkuser'@'%' identified by 'xxxxxxxxxxxxxx';

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 7]

# **6.登录系统** #

http://192.168.1.15:15111

账号：admin

密码：admin

登录上去之后，会报出一些说有危险之类的警告，那你就按她的要求修改吧，免得她又发什么神经。

\# cat /usr/local/wwwweb/testlink/config.inc.php

$tlCfg->config\_check\_warning\_mode = 'FILE';

改为：

$tlCfg->config\_check\_warning\_mode = 'SILENT';

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 8]

修改账号密码：

管理员账号密码

admin

xxxxxxxxxx

![testlink测试用例系统搭建完善文档，一个不错的测试管理工具][testlink 9]

# **7.后话** #

文章属原创，未经允许，请尊重原创，请勿在其他地方抄袭发表。

写文章不易，我会坚持更新，希望大家多多关注点赞，如果有什么想法，或者想我出什么类型什么内容的文章，可以在文章下方评论，我会尽我所能满足大家的要求，谢谢。


[testlink]: /pro/os/crawler/3ENA-ZAZM-U3QA.jpg
[testlink 1]: /pro/os/crawler/IYN7-BJAI-VUIZ.jpg
[testlink 2]: /pro/os/crawler/6NJV-6VNR-EEEJ.jpg
[testlink 3]: /pro/os/crawler/2EIB-Z2AR-YN7Z.jpg
[testlink 4]: /pro/os/crawler/VVNM-EQI3-IARM.jpg
[testlink 5]: /pro/os/crawler/EJYM-QMJN-BNAY.jpg
[testlink 6]: /pro/os/crawler/ZYJ6-RVRM-3IYV.jpg
[testlink 7]: /pro/os/crawler/VRVI-7NZB-EUYE.jpg
[testlink 8]: /pro/os/crawler/MZYF-B3QF-UMUF.jpg
[testlink 9]: /pro/os/crawler/VYNA-FFBY-J7FU.jpg
 *  **原文作者：** 运维人生精选
 *  **原文链接：** https://www.toutiao.com/item/6517791942447727111/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。