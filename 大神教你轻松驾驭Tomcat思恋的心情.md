---
title: 大神教你轻松驾驭Tomcat
date: 2018-04-04 06:32:29
categories: "开发"
tags:
	- 文本编辑器
	- Java
	- Tomcat
	- 编程语言
	- WebApp

---

**Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。**


![大神教你轻松驾驭Tomcat][Tomcat]

对于一个初学者来说，可以这样认为，当在一台机器上配置好Apache 服务器，可利用它响应HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上Tomcat是Apache 服务器的扩展，但运行时它是独立运行的，所以当你运行tomcat 时，它实际上作为一个与Apache 独立的进程单独运行的。

诀窍是，当配置正确时，Apache 为HTML页面服务，而Tomcat 实际上运行JSP 页面和Servlet。另外，Tomcat和IIS等Web服务器一样，具有处理HTML页面的功能，另外它还是一个Servlet和JSP容器，独立的Servlet容器是Tomcat的默认模式。不过，Tomcat处理静态HTML的能力不如Apache服务器。目前Tomcat最新版本为9.0。

一、Tomcat体系架构

大神教你轻松驾驭Tomcat大神教你轻松驾驭Tomcat

核心组件

``````````
server：相当于一个tomcat实例。接收并解析请求信息；完成相关动作后把响应结果返回给计算机。service：每个server包含多个service，相互独立，仅共享JVM以及类库。用于把连接器(connector)与引擎(engine)关连起来,且一个service只能有一个engine，但是可以有多个connector。因为engine无法直接接受连接器发来的数据。connector：负责开启socket并监听客户端请求、返回响应数据。多个connector监听多个端口engine：负责具体的处理请求，connector仅仅负责监听，收到数据后交给engine运行。host：在ngine中可以包含多个host，每个host定义了虚拟主机context：每个context可以部署一个web应用。一个host可以存在多个context。如果部署多个应用需要分别对每个应用装载所依赖的库，这个步骤可以自动可以手动。
``````````

二、Tomcat的安装

tomcat其实就是一个JAVA程序，所以要运行在JAVA虚拟机中。要运行虚拟机就要先安装JDK。

1.JDK的安装

**1.通过YUM安装**

``````````
yum install java-1.8.0-openjdk-devel
``````````

**2.配置环境变量**

``````````
vim /etc/profile.d/java.shexport JAVA_HOME=/usr/java/latest # 首先定义JAVA_HOME的环境变量export PATH=$JAVA_HOME/bin:$PATH # 然后向后追加即可
``````````

2.Tomcat的安装

首先要从Tomcat的官网下载Tomcat，然后上传至服务器解压。 https://tomcat.apache.org

**1.将下载的软件包解压**

``````````
tar xf apache-tomcat-VERSION.tar.gz -C /usr/local/cd /usr/local
``````````

**2.创建软连接，或者将解压的tomcat直接改名为tomcat也能达到同样的效果**

``````````
ln -sv apache-tomcat-VERSION tomcat
``````````

**3.添加环境变量**

``````````
vim /etc/profile.d/tomcat.shexport CATALINA_BASE=/usr/local/tomcatexport PATH=$CATALINA_BASE/bin:$PATH
``````````

**4.创建tomcat需要的用户**

``````````
useradd tomcat
``````````

**5.将安装包的路径下的所有属组都改为tomcat**

``````````
chown -R root.tomcat /usr/local/tomcat # 设定所有者为root，所属组为tomcatchown -R tomcat /usr/local/tomcat/{logs,temp,work,webapps} # 仅将需要有写权限文件所有者改为tomcatchmod g+r /usr/local/tomcat/conf # 默认没有权限，会导致启动失败
``````````

**6.切换到tomcat用户最后启动服务即可**

``````````
su - tomcatcatalina.sh start # 启动tomcat。catalina.sh命令需要先添加环境变量
``````````

三、Tomcat服务的配置文件结构

> bin： 脚本、以及Tomcat自身所携带的工具包
> 
> conf： Tomcat服务的配置文件目录；
> 
> lib： 库文件，Java类库，jar；
> 
> logs： 日志文件目录；
> 
> temp： 临时文件目录；
> 
> webapps：webapp的默认目录；相当于页面的根目录。部署的应用都应该在IC目录下
> 
> work： 工作目录，存放编译后的字节码文件；

四、部署测试页面

创建一个测试页面，将下面的步骤全部做完后，通过浏览器访问http://IP:8080/test 即可访问到测试的页面

**1.创建文件夹**

``````````
classes、lib、WEB-INF为一个标准应用应该有的目录，这里创建仅仅为了与标准看齐mkidr -pv /usr/share/tomcat/webapps/test/{classes,lib,WEB-INF}
``````````

**2.创建一个测试页面用于检验Tomcat服务是否能正常提供服务**

**3.重启服务**

``````````
systemctl restart tomcat
``````````

**4.部署完成后在webapp目录自动生成一些目录**

``````````
cd /usr/share/tomcat/work/Catalina # 部署完成后自动生成的test目录下的文件[root@localhost Catalina]# tree ..└── localhost # 默认主机站点 ├── _ ├── docs ├── examples ├── host-manager ├── manager ├── sample └── test # webpp应用名称。自动生成以下目录 └── org └── apache └── jsp ├── index_jsp.class └── index_jsp.java
``````````


[Tomcat]: /pro/os/crawler/YIUY-FVIF-BRZQ.jpg
 *  **原文作者：** 思恋的心情
 *  **原文链接：** https://www.toutiao.com/item/6540173627323580935/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。