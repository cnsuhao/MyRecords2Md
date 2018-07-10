---
title: ajax的post提交参数长度超出限制的解决办法
date: 2017-05-15 22:16:44
categories: "开发"
tags:
	- ajax

---

tomcat 下 post提交默认最大 2M，修改maxPostSize值可解决参数长度超出限制。

修改tomcat文件目录下 conf/service.xml 文件

![MI2A-22QA-BR6B.jpg][]


**tomcat 版本 >= 7.0.63** maxPostSize 必须小于 0 ，否则会出现 参数值 为null 的情况。

**tomcat 版本 < 7.0.63** 则 maxPostSize <=0 均可实现解决post提交超出限制的问题。

**综上所述 ： 设置 maxPostSize = -1 即可解决tomcat不同版本下 post 提交 最大长度限制。**

当然，如果设置正数值，只要保证在业务允许范围内，亦可解决该问题。【推荐】




[MI2A-22QA-BR6B.jpg]: /pro/os/crawler/MI2A-22QA-BR6B.jpg