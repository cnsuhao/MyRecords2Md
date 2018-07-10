---
title: 败给Kotlin，Java到底弱在哪儿？
date: 2017-10-27 10:57:02
categories: "开发"
tags:
	- Java
	- Google
	- Scala
	- 编程语言
	- Kotlin

---

前不久Google宣布在Google I / O 上宣布为Kotlin提供Android官方支持，这也意味着Java今后将告别Android开发的舞台。那么，为何是Kotlin，Java到底弱在哪儿?本文将带你解读Android社区选择Kotlin的几大理由!

![败给Kotlin，Java到底弱在哪儿？][Kotlin_Java]

**Kotlin VS Java**

早在Google I / O公布Kotlin在Android领域将取代Java以前，2016年2月发布的Kotlin 1.0版本Android社区就已经提出了这个问题。这是因为Kotlin代码不仅比Java更安全，还更简洁。而且Kotlin和Java文件在Android应用程序中是可以共存的，所以Kotlin不仅可以应用于新程序的开发，还可以对现有的Java应用程序进行扩展。

此外，对于Android开发程序员来说这也可能是一个很大的福利——大多数的Android文档和示例都是关于Java的。另一方面，在Android Studio中将Java转换为Kotlin也非常简单，只需将Java代码粘贴到Kotlin文件中即可。

Kotlin的优势显而易见，资深的Java开发人员要学习Kotlin也只需要几个小时的时间就能搞定。而且学习的范围也仅仅是包括消除空参考错误、启用扩展功能、支持编程功能以及添加协同工作等。

![败给Kotlin，Java到底弱在哪儿？][Kotlin_Java 1]

**　Kotlin VS Scala**

由于Android开发工具对Scala支持还有一定的障碍，而且Scala的Android库也是很难搞定的一个问题，所以Android社区最终选择了Kotlin。另一方面，Scala社区也已经明显意识到了这个问题，正在努力的解决这些问题。

但对于Scala本身是不能否定的。因为不同的环境中应用到的语言和系统工具可能也不相同。例如，Apache Spark主要是用Scala编写的，Spark的大数据应用程序通常也会用到Scala。

在许多方面，Scala和Kotlin代表着面向对象语言的融合。这两种语言许多的概念和符号都是共享的，例如不可声明的val与可变声明var。但是在某些方面比方说在声明lambda函数时的是设置双箭头还是单箭头的问题还是略有不同。

但是Scala有一个明显的缺陷是编译时间往往很长，开发人员如果要构建一个庞大的Scala存储库时，这个问题会更加明显。而Kotlin是在最常见的软件场景中被设计的，它的编译速度比Java代码编译都快，堪称秒杀Scala。

**Kotlin与Java的互操作性**

谈到null处理和检查异常的差异问题，你可能会想知道Kotlin对Java处理互操作性调用结果的处理。Kotlin推出的平台类型与Java类型完全类似，这就意味着可以为空但会生成空指针异常。Kotlin也可能在编译时向代码中注入一个断言，以避免触发实际的空指针异常。平台类型没有明确的语言符号，但是如果Kotlin必须报告平台类型。

在大多数情况下，从Kotlin调用的代码也可以按照程序员期望的方式工作。此外Kotlin与Java的互操作性涉及到Java工具方面上——Kotlin没有自己的编译器或IDE，但是有流行的Java编译器和IDE插件，包括IntelliJ IDEA，Android Studio和Eclipse。也没有自己的构建系统，使用的是Gradle、Maven和Ant等。

![败给Kotlin，Java到底弱在哪儿？][Kotlin_Java 2]

总体来讲，Kotlin比Java拥有更多的优势。不仅可以在JVM上运行代码，还可以生成JavaScript和本机代码。与Java相比，Kotlin更安全、简洁明确。除了面向对象编程之外，它还支持功能编程，提供扩展功能、构建器、协同程序等。重要的是，对于已经熟悉Java的程序员来讲无需浪费太多的时间学习Kotlin。


[Kotlin_Java]: /pro/os/crawler/AUEZ-INMY-B7FB.jpg
[Kotlin_Java 1]: /pro/os/crawler/FFRU-6J36-ZAZV.jpg
[Kotlin_Java 2]: /pro/os/crawler/JANU-QAR6-NIZM.jpg
 *  **原文作者：** IT168企业级
 *  **原文链接：** https://www.toutiao.com/item/6481419278565245454/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。