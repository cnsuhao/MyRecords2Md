---
title: Java本地的项目，怎么可以让别人通过外网访问-内网穿透
date: 2017-09-26 23:47:30
categories: "开发"
tags:
	- Java
	- 程序员
	- Tomcat
	- 编程语言
	- Windows

---

# 一、点击链接 https://natapp.cn/ 注册个免费的账户 #

![Java本地的项目，怎么可以让别人通过外网访问-内网穿透][Java_-]

NATAPP官网

# 二、登陆进去以后查看authtoken。复制这个，等下要在客户端用到！ #

![Java本地的项目，怎么可以让别人通过外网访问-内网穿透][Java_- 1]

分配的authtoken

# 三、点击个人中心，稍微做一下配置： #

![Java本地的项目，怎么可以让别人通过外网访问-内网穿透][Java_- 2]

简单的配置

# 四、点击官网的立即下载 下载windows x64位的客户端 #

![Java本地的项目，怎么可以让别人通过外网访问-内网穿透][Java_- 3]

下载

# 五、windows 系统下 双击这个出现这个界面 #

![Java本地的项目，怎么可以让别人通过外网访问-内网穿透][Java_- 4]

启动

# 六、然后用这个命令登录 #

natapp -authtoken=ad6b3f11eb72f44b

启动成功的界面：

![Java本地的项目，怎么可以让别人通过外网访问-内网穿透][Java_- 5]

启动成功

# **七、可以看到 那个http://pebq83.natappfree.cc域名映射的是127.0.0.1：8080** #

**然后tomcat 部署自己的项目，启动以后，****用http://pebq83.natappfree.cc域名加上自己项目名称就可以外网访问了**

**例如：http://pebq83.natappfree.cc/gmfSport/**

# 好了，今天的技术内容就分享给大家，码农不容易，小编给大家分享写博客也不容易，请多多支持，喜欢请关注头条号，每天都有功能和bug分享给大家一起学习进步，我们的目标是 ----软件攻城狮 #

# 有兴趣的朋友一起学习java 可以加群---《专注JavaWeb开发》 群号：162582394 #

# 码农不容易，码文章更不容易啊，喜欢小编多多支持请点击关注哦，小编会更加努力每天给大家分享技术文章。 #


[Java_-]: /pro/os/crawler/UFJY-RYJ6-FIIA.jpg
[Java_- 1]: /pro/os/crawler/MYJV-URRV-EIIE.jpg
[Java_- 2]: /pro/os/crawler/IYAR-7NYV-NM3Q.jpg
[Java_- 3]: /pro/os/crawler/EEM6-VVZY-MNUE.jpg
[Java_- 4]: /pro/os/crawler/7JF6-RRZE-YFVJ.jpg
[Java_- 5]: /pro/os/crawler/NRNZ-RNUY-YYJ2.jpg
 *  **原文作者：** 专注JavaWeb开发
 *  **原文链接：** https://www.toutiao.com/item/6470112787753337358/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。