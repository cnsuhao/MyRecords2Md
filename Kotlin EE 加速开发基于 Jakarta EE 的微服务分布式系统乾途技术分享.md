---
title: Kotlin EE 加速开发基于 Jakarta EE 的微服务分布式系统
date: 2018-05-11 17:44:34
categories: "开发"
tags:
	- Java虚拟机
	- Eclipse
	- JavaScript
	- 编程语言
	- Kotlin

---

![Kotlin EE 加速开发基于 Jakarta EE 的微服务分布式系统][Kotlin EE _ Jakarta EE]

Kotlin 是一款基于 JVM 的编程语言，与 JVM 100%兼容，目前被谷歌定为安卓官方开发语言之一。Jakarta EE 是 Java EE 从 Oracle 剥离出来到 Eclipse 基金会接手时经历众多投票阶段定下来的名字，现正向小而简且适合进行快速微服务开发的方向进行。

Kotlin 的语言十分简洁和灵活，利用当今流行的函数式编程模型，减少诸多冗余代码，并且产生了一系列新的编程模式。如今 Kotlin 与 Jakarta EE 结合，可以实现减少巨大数量的代码行数。小编曾使用 Kotlin 尝试开发过基于 JAX-RS 的 web demo，也开发过基于 Hibernate OGM 实现 NoSQL 到 Object 映射的 demo。现贴上部分代码，来看看代码的简洁程度：

![Kotlin EE 加速开发基于 Jakarta EE 的微服务分布式系统][Kotlin EE _ Jakarta EE 1]

因为是测试代码，所以把数据库操作写到了 Rest 资源类中，仅供展示用。Kotlin 还有官方开发的 Ktor Web 开发框架，基于 Netty 实现了高性能的异步编程模型，有兴趣的可自行去官网查看。


最近，Marcus Fihlon 给的一段 Kotlin EE：加速你的生产力 演讲中，带动了 Kotlin 进行 Jakart EE 应用开发的热度，并且引起了 Java EE guardians 社区的关注。

另外 Kotlin 还能够生成 JavaScript 代码，并且还支持基于 LVVM 的本地应用。可见 Kotlin 的强大之处还有广阔的发展前景。


[Kotlin EE _ Jakarta EE]: /pro/os/crawler/BUJN-MI3M-22EE.jpg
[Kotlin EE _ Jakarta EE 1]: /pro/os/crawler/2EQM-J3VM-IIQE.jpg
 *  **原文作者：** 乾途技术分享
 *  **原文链接：** https://www.toutiao.com/item/6554256994587378190/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。