---
title: Kotlin和Java程序员福利：轻量级Web框架Javalin
date: 2017-11-09 14:28:41
categories: "开发"
tags:
	- Java
	- 程序员
	- 编程语言
	- Spark
	- Kotlin

---

Javalin是一款非常适合Kotlin和Java程序员的轻量级Web框架，它第一个版本是今年6月份发布的，目前刚刚达到稳定版本的Javalin1.0.0.0。Javalin主要有以下的特点：

![Kotlin和Java程序员福利：轻量级Web框架Javalin][Kotlin_Java_Web_Javalin]

·轻量级：不用提前学习任何概念就可以开始使用

·一致的API：所有的处理程序和映射器在Context (ctx)中都是无效的。

·Kotlin和Java拥有几乎完全相同的API

·是框架也是库：无需扩展任何功能

·拥有完全可定制的嵌入式服务器(Jetty)

·JSON对象映射

·通过AccessManager 接口简单的按端点验证

·简单的静态文件处理

·生命周期事件

·CookieStore——一种简单的用来序列化的方法和存储在cookie中的对象。

·模板渲染

·Markdown渲染

此外，如果Javalin 0.5.X版本升级到1.0.0，不会造成任何的破坏。

![Kotlin和Java程序员福利：轻量级Web框架Javalin][Kotlin_Java_Web_Javalin 1]

**Javalin：是框架也是库**

轻量级Kotlin和Java 的Web框架受到Sparkjava与koa.js的启发。Javalin主要是用Kotlin编写的，Java参与了几个功能接口的编写，这可能会使得Kotlin和Java程序员拥有非常类似的体验。此外，它是一个框架，也是一个库。学习Javalin的最大好处就是无需扩展或实施任何东西就可直接使用。

Javalin最初是Spark Java和Kotlin Web框架的一个分支，但随着koa.js的倒闭，只得进行重写。所有Web框架都受到了现代微网络框架之父Sinatra的启发，如果你来自Ruby，对Javalin应该不会感到陌生。

Javalin的目标是成为一个轻量级的REST API库。虽然没有MVC概念，但为了方便它还支持模板引擎、websockets和静态文件服务，程序员可以使用Javalin来创建RESTful API后端、为index.html 静态资源提供服务 。

**Hello World**

Kotlin

![Kotlin和Java程序员福利：轻量级Web框架Javalin][Kotlin_Java_Web_Javalin 2]

Java

![Kotlin和Java程序员福利：轻量级Web框架Javalin][Kotlin_Java_Web_Javalin 3]

Javalin在设计的时候考虑到Kotlin和Java之间的互操作性，所以如果将Javalin项目从Java移植到Kotlin时就会很简单。如果你之前用过Javalin，那应该明白Kotlin与Java切换也没有那么麻烦。此外，为Kotlin和Java维护一致的API也是Javalin的一个重要目标。


[Kotlin_Java_Web_Javalin]: /pro/os/crawler/YFZI-BAUQ-FYQZ.jpg
[Kotlin_Java_Web_Javalin 1]: /pro/os/crawler/MZEE-VYZF-UIMQ.jpg
[Kotlin_Java_Web_Javalin 2]: /pro/os/crawler/IBI7-NFRQ-EB7R.jpg
[Kotlin_Java_Web_Javalin 3]: /pro/os/crawler/7RB6-ZRQQ-FNEN.jpg
 *  **原文作者：** IT168企业级
 *  **原文链接：** https://www.toutiao.com/item/6486297923352003085/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。