---
title: Javascript高级程序设计1-2章回顾总结
date: 2012-10-15 21:08:14
categories: "开发"
tags:
	- Java

---

第一章：JavaScript简介

1、Javascript是什么？

JavaScript是一种专为与网页交互而设计的脚本语言。

2、JavaScirpt组成部分。

JavaScript由三部分组成：ECMAScript、DOM、BOM。

ECMAScript:是Javascript的核心，由ECMS-262定义，提供核心核心语言功能。

ECMA-262规定了这门语言的下列组成部分：

语法、类型、语句、关键字、保留字、操作符、对象

ECMAScript就是对实现该标准规定的各个方面内容的语言的描述。Javascript实现了ECMAScirpt, Adobe ActionScript和Open View ScriptEase同样也实现了ECMAScript.

如图：

|----JavaScript

ECMAScript---|----ActionScript

|---ScriptEase

\------------------------------------------------------------------------------------------------------------------------------------------------------------------------

DOM：document object model（文本对象模型）：是针对xml但经过扩展用于HTML的应用程序编程接口（API,Application Programming Interface）.


\------------------------------------------------------------------------------------------------------------------------------------------------------------------------

BOM: browser object model(浏览器对象模型)：提供与浏览器交互的方法和接口。


\--------------------------------------------------------------------------------------------------------------------------------------------------

各大浏览器提供商均不同程度上实现对Javascript的支持。 提供谷歌的V8 Javascript引擎地址：http://code.google.com/p/v8/ 网上说均由C++实现。具体的没看，V8确实是。

================================================================================================================================

第二章 在HTML中使用Javascript

第二章主要讲述了再HTML中如何使用Javascript，以及使用Javascript所注意的细节、语法等。不过勾起我兴趣的是以下这段话：

**按照惯例，外部Javascript文件带有js扩展名，但这个扩展名不是必需的，因为浏览器不会坚持包含Javascirpt的文件扩展名，这样一来使用JSP、php或其他服务器端语言动态生成Javascript代码也就成为了可能。**

该段话值得研究~~

//以上文字纯手敲，错误难免~![微笑][QJVY-NZEN-YNZ2.gif]




[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif