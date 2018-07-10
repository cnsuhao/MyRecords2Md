---
title: 手把手带你搭建Jenkins持续集成
date: 2017-10-25 07:45:15
categories: "开发"
tags:
	- 脚本语言
	- Tomcat
	- 技术
	- Linux

---

# Jenkins搭建以及配置（此处是Linux教程，window的和这个一样） #

**总共分为四大部分：**

# 一、安装 #

**1****、将War****包直接部署到tomcat****中，war****包下载地址：**

> https://jenkins.io/download/

**2、****访问页面，会提示输入密码**

**3、****密码查看方法：**

> Linux：
> 
> cat /root/.jenkins/secrets/initialAdminPassword
> 
> windows：
> 
> 在User下的一个隐藏文件夹.jenkins下

**4、****输入完密码若报错，则下载cloudbees-folder.hpi,**

**放到tomcat****下webapps/jenkins/WEB-INF/detached-plugins****下并重启tomcat****即可。**

**5、****插件安装**

![手把手带你搭建Jenkins持续集成][Jenkins]

插件

**6、****安装完插件后会进入创建用户界**

![手把手带你搭建Jenkins持续集成][Jenkins 1]

创建用户

可以创建也可以直接跳过，直接用admin也行。

7、**成功后进入主页面**

# 二、配置 #

1、点击系统管理

![手把手带你搭建Jenkins持续集成][Jenkins 2]

系统管理

2、进行全局配置

![手把手带你搭建Jenkins持续集成][Jenkins 3]

全局配置

3、配置JDK和Maven

![手把手带你搭建Jenkins持续集成][Jenkins 4]

配置JDK

![手把手带你搭建Jenkins持续集成][Jenkins 5]

配置maven

**Save**

4、新建项目

![手把手带你搭建Jenkins持续集成][Jenkins 6]

新建项目

5、配置SVN

![手把手带你搭建Jenkins持续集成][Jenkins 7]

配置SVN

6、构建脚本

![手把手带你搭建Jenkins持续集成][Jenkins 8]

构建脚本

![手把手带你搭建Jenkins持续集成][Jenkins 9]

maven脚本

点击**增加构建步骤****,****构建一个shell****，我们需要在这里写shell****脚本（部署到tomcat****）**

![手把手带你搭建Jenkins持续集成][Jenkins 10]

这里写shell脚本

由于脚本不能通用，所以这里不贴了。

# 三、构建 #

1、配置完后在首页会出现如下

![手把手带你搭建Jenkins持续集成][Jenkins 11]

首页

2、对此项目进行构建

![手把手带你搭建Jenkins持续集成][Jenkins 12]

构建

# 四、其他 #

Jenkins功能很强大，可以创建多用户，为每个用户配置邮件，这样在build的时候如果失败了，就可以自动发送邮件给xxx（自己配置的收件人）。还可以安装各种插件等。


[Jenkins]: /pro/os/crawler/JBIA-FIR6-FI22.jpg
[Jenkins 1]: /pro/os/crawler/6JJR-IIJV-RRFY.jpg
[Jenkins 2]: /pro/os/crawler/ZYFR-NZUE-R3IU.jpg
[Jenkins 3]: /pro/os/crawler/VI36-7RFB-YJAZ.jpg
[Jenkins 4]: /pro/os/crawler/FRYQ-N2UB-MVQ3.jpg
[Jenkins 5]: /pro/os/crawler/MEMN-6NRU-AIFE.jpg
[Jenkins 6]: /pro/os/crawler/BUIU-UR6F-FYYV.jpg
[Jenkins 7]: /pro/os/crawler/YVQF-YVVR-YIAR.jpg
[Jenkins 8]: /pro/os/crawler/RBY2-AYVE-Y6FF.jpg
[Jenkins 9]: /pro/os/crawler/EN3Y-UJJN-ZNQE.jpg
[Jenkins 10]: /pro/os/crawler/7J2Q-VRI6-VNVQ.jpg
[Jenkins 11]: /pro/os/crawler/QV3M-Z3UJ-63QM.jpg
[Jenkins 12]: /pro/os/crawler/JVYE-VIU6-JFEZ.jpg
 *  **原文作者：** 编程界的小学生
 *  **原文链接：** https://www.toutiao.com/item/6479623114270441997/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。