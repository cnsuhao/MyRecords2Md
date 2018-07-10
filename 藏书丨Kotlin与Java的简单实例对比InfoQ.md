---
title: 藏书丨Kotlin与Java的简单实例对比
date: 2017-09-28 09:19:31
categories: "开发"
tags:
	- Java
	- 程序员
	- 编程语言
	- 程序设计
	- Kotlin

---

在快速发展与创新的今天，不断孕育出各种新语言。Kotlin 非常具有代表性，具有简明性和独特的表达能力，同时易于“并发编程”。Kotlin 的优势体现在哪里？为何 Java 程序员要转向 Kotlin？

下面我们就针对程序设计中的一些基本功能，同时使用 Java 与 Kotlin 来写代码，看看效果会是什么样的。

1. 打印日志

其实，Kotlin 中的 println 函数是一个内联函数，就是通过封装 java.lang.System 类的 System.out.println 来实现的：

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java]

    @kotlin.internal.InlineOnly public inline fun print(message: Any?) { System.out.print(message) }

2. 常量与变量

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 1]

3. 声明

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 2]

4. 空判断

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 3]

在 Kotlin 中，只使用一个问号安全调用符号就省去了 Java 中烦人的 if - 判断。

5. 字符串拼接

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 4]

Kotlin 中使用 $ 和 $\{\}（花括号里面是表达式的时候）占位符来实现字符串的拼接，这比在 Java 中每次使用加号来拼接要方便许多。

6. 换行

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 5]

7. 三元表达式

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 6]

8. 操作符

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 7]

9. 类型判断和转换（显式）

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 8]

10. 类型判断和转换 (隐式)

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 9]

Kotlin 的类型系统具备一定的类型推断能力，这样也省去了不少在 Java 中类型转换的样板式代码。

11.Range 区间

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 10]

12. 更灵活的 case 语句

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 11]

13.for 循环

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 12]

14. 更方便的集合操作

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 13]

15. 遍历

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 14]

16. 方法 (函数) 定义

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 15]

17. 带返回值的方法（函数）

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 16]

Kotlin 中的函数可以直接传入函数参数，同时可以返回一个函数类型。

18.constructor 构造器

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 17]

19.JavaBean 与 Kotlin 数据类

这段 Kotlin 中数据类的代码如下：

    data class Developer(val name: String, val age: Int)

对应下面这段为 Java 实体类的代码：

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 18]![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 19]![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 20]

通过这些对比，我们能感受到 Kotlin 的简洁、优雅，可用更少的代码来实现更多的功能。 另外，在 IDEA 中，可以直接使用 Kotlin 插件进行 Java 代码与 Kotlin 代码之间的转换。

Kotlin 的定位之一就是官网首页重点强调的：100% interoperable with Java。在 Java 生态领域最广为人知的 Spring 框架，在最新的 Spring 5 中对 Kotlin 也有了支持。

文章摘自《Kotlin 极简教程》

![藏书丨Kotlin与Java的简单实例对比][Kotlin_Java 21]

    《Kotlin 极简教程》 ISBN：9787111579939 作者：陈光剑 著 定价：79.00 元

阿里 Java 程序员撰写，带你快速进入 Kotlin 的世界，零基础学会 Kotlin 开发。基于 Kotlin 1.1 版本，从 Kotlin 基础知识到动手实战，包含大量精选示例代码和应用案例。


[Kotlin_Java]: /pro/os/crawler/RBQQ-UY3Y-QJE2.jpg
[Kotlin_Java 1]: /pro/os/crawler/Q7NN-UB3E-FBZQ.jpg
[Kotlin_Java 2]: /pro/os/crawler/73UQ-RRVA-F2MY.jpg
[Kotlin_Java 3]: /pro/os/crawler/EAQ3-AQUB-AV7Z.jpg
[Kotlin_Java 4]: /pro/os/crawler/FJUE-7B6F-J7VB.jpg
[Kotlin_Java 5]: /pro/os/crawler/Q32I-6NZN-MMMI.jpg
[Kotlin_Java 6]: /pro/os/crawler/VVIB-3UJ7-JU73.jpg
[Kotlin_Java 7]: /pro/os/crawler/QB3Q-EBFB-MBN2.jpg
[Kotlin_Java 8]: /pro/os/crawler/NR7F-BQVQ-6ZUF.jpg
[Kotlin_Java 9]: /pro/os/crawler/6RRQ-VNUZ-IBFE.jpg
[Kotlin_Java 10]: /pro/os/crawler/BYUZ-RARA-UJY3.jpg
[Kotlin_Java 11]: /pro/os/crawler/RR2E-NJZU-AUVM.jpg
[Kotlin_Java 12]: /pro/os/crawler/VZIF-UQJA-MYUB.jpg
[Kotlin_Java 13]: /pro/os/crawler/ENIU-2URB-3IYR.jpg
[Kotlin_Java 14]: /pro/os/crawler/I7F7-FJEU-RIF2.jpg
[Kotlin_Java 15]: /pro/os/crawler/UEFN-IUYR-FABE.jpg
[Kotlin_Java 16]: /pro/os/crawler/7VJJ-AIBM-FQYZ.jpg
[Kotlin_Java 17]: /pro/os/crawler/QVAV-QNQM-6V2M.jpg
[Kotlin_Java 18]: /pro/os/crawler/J3YJ-AUB6-JV32.jpg
[Kotlin_Java 19]: /pro/os/crawler/ZBEF-N2YE-BVUY.jpg
[Kotlin_Java 20]: /pro/os/crawler/QFEM-BERU-ZNUN.jpg
[Kotlin_Java 21]: /pro/os/crawler/QARV-BA7Z-ZJEV.jpg
 *  **原文作者：** InfoQ
 *  **原文链接：** https://www.toutiao.com/item/6470632694488236558/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。