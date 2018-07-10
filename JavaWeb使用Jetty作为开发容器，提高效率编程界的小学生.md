---
title: JavaWeb使用Jetty作为开发容器，提高效率
date: 2017-11-02 09:33:26
categories: "开发"
tags:
	- Tomcat
	- 技术
	- JSP
	- WebApp

---

说明：依旧使用tomcat作为容器，Jetty在这里只作为开发时测试方便，所以采取工具类形式，而非下载Jetty容器包配置方式。是开发容器，（非线上或测试部署容器）

那么为什么要用Jetty？

此处用Jetty只是方便测试，运行个main方法即可启动Jetty容器，然后就可以调用接口和访问jsp等页面。无需启动tomcat，个人认为Jetty启动速度远快于tomcat。所以方便测试，更改代码只需要重新运行main方法即可。（Jetty自带热部署）

怎么用？

pom.xml定义如下  


![JavaWeb使用Jetty作为开发容器，提高效率][JavaWeb_Jetty]

工具类

![JavaWeb使用Jetty作为开发容器，提高效率][JavaWeb_Jetty 1]

![JavaWeb使用Jetty作为开发容器，提高效率][JavaWeb_Jetty 2]

![JavaWeb使用Jetty作为开发容器，提高效率][JavaWeb_Jetty 3]

![JavaWeb使用Jetty作为开发容器，提高效率][JavaWeb_Jetty 4]

主方法

![JavaWeb使用Jetty作为开发容器，提高效率][JavaWeb_Jetty 5]

解释说明：

pom和工具类直接复制粘贴

主方法

String webapp = "src/main/webapp";： 是当前web项目的webapp这层目录结构（因为里面包含了class和页面等）

new JettyServerStarter(webapp, 8012, "/clinic-demo").start(); new 一个Jetty容器，第一个参数是webapp地址，第二个参数是端口号(自定义)，第三个参数是当前web项目名称。

访问路径：http://localhost:8012(自定义Jetty的端口)/clinic-demo


[JavaWeb_Jetty]: /pro/os/crawler/QNRJ-JMII-UIBI.jpg
[JavaWeb_Jetty 1]: /pro/os/crawler/3YZV-QUYZ-YZAF.jpg
[JavaWeb_Jetty 2]: /pro/os/crawler/6NIE-YAMN-EVAU.jpg
[JavaWeb_Jetty 3]: /pro/os/crawler/E2QU-J2MV-UN22.jpg
[JavaWeb_Jetty 4]: /pro/os/crawler/B3QY-NQNQ-7RVV.jpg
[JavaWeb_Jetty 5]: /pro/os/crawler/IAYE-FVZU-IQ7Z.jpg
 *  **原文作者：** 编程界的小学生
 *  **原文链接：** https://www.toutiao.com/item/6483615994433503757/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。