---
title: JVM中微服务的未来，轻量级和反应式开发框架Micronaut
date: 2018-05-23 15:18:15
categories: "开发"
tags:
	- Java虚拟机
	- Neo4J
	- Docker
	- MongoDB
	- Gradle

---

Micronaut是专为微服务设计，它是JVM上轻量级和反应式开发的框架。本文将解释Micronaut如何占用最小空间来帮助加快你的微服务实现。

![JVM中微服务的未来，轻量级和反应式开发框架Micronaut][JVM_Micronaut]

今年3月，Grails公布了新的Micronaut全栈框架。Micronaut是一个全新的JVM框架，从底层设计到微服务和无服务器计算环境，它能够满足：

 *  快速启动时间
 *  低内存占用
 *  尽可能小的JAR文件
 *  零依赖
 *  12 factor

使用当前版本的Micronaut，最小的JAR文件是8 MB，可以使用最少10mb max heap来运行。如果使用Java，启动时间为亚秒。此外，Micronaut的应用启动时间和内存消耗与代码行数量不成线性增长，因为所有依赖注入，AOP和代理生成都发生在编译时。

Micronaut是基于Netty的超轻量级和反应式的，同时提供HTTP客户端和服务器。如下中的下一个代码显示了在Micronaut中声明的HTTP Server端点以及在编译时生成实现的客户端。

![JVM中微服务的未来，轻量级和反应式开发框架Micronaut][JVM_Micronaut 1]

如你所见，以前的代码清单提供了与Spring Boot类似的编程模型或Grails。来自这些框架的开发人员正在寻找一个在分布式架构中使用的框架，将享受快速而熟悉的学习曲线。这将使你能够轻松地从已有的应用程序转换到Micronaut。

此外，Micronaut还提供专门针对微服务/云的功能：

 *  服务发现
 *  断路器/重试
 *  配置共享
 *  客户端负载平衡
 *  支持无服务器计算——AWS Lambda

所有这些功能都内置了零附加依赖项。例如，该框架包含一个实现Circuit Breaker模式的@CircuitBreaker注释。

![JVM中微服务的未来，轻量级和反应式开发框架Micronaut][JVM_Micronaut 2]

之前的代码清单将5配置为重试尝试的最大次数，尝试之间的延迟为5毫秒，在将circuit重置为半打开之前设置为300毫秒。

@CircuitBreaker在另一个baked-in的Micronaut的注释中扮演着很好的角色：@Fallback。一个注释，它允许你定义回退Bean，以及可用于为尝试在测试环境中覆盖Bean的宕机时间的微服务提供替代方法的注释。

如下是一个“pet store”应用程序的演示，该应用程序被设计为多项目Gradle应用程序。该应用程序说明了Micronaut微服务可以轻松地与不同的持久性机制（如Neo4j，Redis，Postgresql，MongoDB）轻松集成，轻松对话第三方服务，如SendGrid或AWS SES，甚至可以部署在无服务器场景中，例如AWS Lambda 。Consul被用作服务发现服务。应用程序中涉及的微服务可以手动启动，也可以使用Docker单个命令启动。

下图说明了演示应用程序：

![JVM中微服务的未来，轻量级和反应式开发框架Micronaut][JVM_Micronaut 3]

据悉，Micronaut的第一个里程碑版本预计将在2018年第二季度推出。


[JVM_Micronaut]: /pro/os/crawler/3UZM-NEBY-RJM3.jpg
[JVM_Micronaut 1]: /pro/os/crawler/IYRM-AF7B-3ERQ.jpg
[JVM_Micronaut 2]: /pro/os/crawler/QVYB-A2UA-QIAR.jpg
[JVM_Micronaut 3]: /pro/os/crawler/JFJR-RBNN-B2EV.jpg
 *  **原文作者：** 云智时代
 *  **原文链接：** https://www.toutiao.com/item/6558672309870133773/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。