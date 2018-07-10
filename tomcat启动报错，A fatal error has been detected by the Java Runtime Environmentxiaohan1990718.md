---
title: tomcat启动报错，A fatal error has been detected by the Java Runtime Environment
date: 2013-01-08 19:02:30
categories: "开发"
tags:
	- Java
	- tomcat

---

折腾了良久，解决了此问题，先前CSDN有位哥们是这么解决的：http://blog.csdn.net/sunzeshan/article/details/7826805

但是我傻，不知道怎么给JDK添加参数，所以该方法没有尝试。以下方法很简单我测试通过了。

错误情况：

\#
\# A fatal error has been detected by the Java Runtime Environment:
\#
\# Internal Error (c1\_Optimizer.cpp:271), pid=1900, tid=4476
\# guarantee(x\_compare\_res != Constant::not\_comparable) failed: incomparable constants in IfOp
\#
\# JRE version: 6.0\_33-b03
\# Java VM: Java HotSpot(TM) Client VM (20.8-b03 mixed mode windows-x86 )
\# An error report file with more information is saved as:
\# D:\\apache-tomcat-6.0.35\\hs\_err\_pid1900.log


并且给了提示：

\#
\# If you would like to submit a bug report, please visit:
\# http://java.sun.com/webapps/bugreport/crash.jsp
\#


翻译即知道是JRE版本的问题。

我同事查了N久发现是JDK6的一个BUG。所以简单的做法是按照JDK7～～

当然是不需要卸载JDK6的，只需要安装到不同文件下即可。并且不需要配置先前的环境变量（先前配置的是6）.

当然如果你希望今后eclipse使用的是JDK7 又不想更改环境变量而影响myEclipse的运行，方法如下：

``````````
修改eclipse目录下的eclipse.ini文件，在第一行(至少在 -vmargs参数之前)添加参数 -vm 指定jdk目录，比如：
-vm
d:\Java\jdk1.7\bin
这样就与你系统环境变量中的设置无关了。
``````````

做法：

安装JDK7后，1、![Image 1][]将程序中的JRE6移除更改为jre7.

![BMIB-RJBI-2IQJ.jpg][]


这样就将你原先程序里的jdk更换为jdk7.

第二步、将tomcat的JVM设置更换为JRE7即可。

![Image 1][]![JAYI-ARMU-FZ7N.jpg][]

OK～～～～收工，运行正常。

![ZB6R-7V3E-Y2M3.jpg][]


![Image 1][]


附注个我以前很傻的认识：JDK本身就是绿色的所以安装版的copy到其他机器上也就行了～～还有出现以上错误的jdk版本是6.0.33 而更换其低版本的jdk貌似也行（我同事是低版本的，没有出现这个bug）。


[Image 1]: 
[BMIB-RJBI-2IQJ.jpg]: /pro/os/crawler/BMIB-RJBI-2IQJ.jpg
[JAYI-ARMU-FZ7N.jpg]: /pro/os/crawler/JAYI-ARMU-FZ7N.jpg
[ZB6R-7V3E-Y2M3.jpg]: /pro/os/crawler/ZB6R-7V3E-Y2M3.jpg