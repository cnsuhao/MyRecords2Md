---
title: jdk--java环境变量设置
date: 2012-06-07 20:15:39
categories: "开发"
tags:
	- java

---

下载jdk后，安装JDK。

JDK安装路径：D:/Java/jdk1.5.0\_04(根据个人情况，我装在D盘)

这里我们首先了解在环境变量中两个符号的用途：

. 表示在当前目录下寻找。

;表示不同路径的分隔符注意不要写成“；”

%JAVA\_HOME%表示名称为JAVA\_HOME的路径。

方式一：

【系统变量】->【新建】 变量名:JAVA\_HOME 变量值：D:/Java/jdk1.5.0\_04(安装路径)

【系统变量】->【新建】 变量名:path 变量值：%JAVA\_HOME%/bin;%JAVA\_HOME%/jre/bin

在系统变量中如果已有classpath不用新建，在变量值的最前面加入

.;%JAVA\_HOME%/lib/tools.jar;%JAVA\_HOME%/lib/dt.jar;

方式二：

path = D:/Java/jdk1.5.0\_04/bin;D:/Java/jdk1.5.0\_04/jre/bin

classpath= D:/Java/jdk1.5.0\_04/lib/dt.jar;D:/Java/jdk1.5.0\_04/lib/tools.jar;

\*PATH里面的路径应尽量放在最前面，例如安装了ORACLE后自带的JDK1.3.1会在最前面导致启动IDE出错！

最后cmd-->javac 即可。