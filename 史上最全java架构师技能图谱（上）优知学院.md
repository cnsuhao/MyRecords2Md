---
title: 史上最全java架构师技能图谱（上）
date: 2017-09-08 10:12:04
categories: "开发"
tags:
	- Java
	- Linux
	- 编程语言
	- 设计模式
	- 大数据

---

![史上最全java架构师技能图谱（上）][java]

> java架构师最全技能图谱上篇，包含：数结构算法、java进阶、web开发、框架与工具四大技能图谱。
> 
> 下篇将包含大数据以及性能、设计模式、UML、中间件、分布式集群、负载均衡、通讯协议、架构设计等技术图谱等章节

本文作者，陈睿 优知学院创始人

优知学院是IT人在线进阶站,帮助IT人升职加薪，导师来自于BAT等一线互联网公司总监。提供系统的互联网产品技术进阶干货资料和课程，以及定期的线下实战活动。

# **一：数据结构算法** #

![史上最全java架构师技能图谱（上）][java 1]

**算法分析**

时间复杂度和空间复杂度

**算法思想**

递推、递归、穷举、贪心、分治、动态规划、迭代、分枝界限

**数据结构**

数组、链表、堆、栈、队列、Hash表、二叉树等

**算法**

排序

经典排序：插入排序、冒泡排序、快排（分划交换排序）、直接选择排序、堆排序、合并排序等

查找

经典查找：顺序查找、二分查找、二叉排序树查找

**高级算法**

贪婪

回溯

剪枝

动态规划

**大数据算法**

hash分桶

统计

# **二：Java进阶** #

![史上最全java架构师技能图谱（上）][java 2]

**java编程基础：**

对象和类 、基本数据类型 、变量类型、运算符、循环分支结构、数组、正则表达式等

**集合**

![史上最全java架构师技能图谱（上）][java 3]

总的说来，Java API中所用的集合类，都是实现了Collection接口，他的一个类继承结构如下：

Collection<--List<--Vector

Collection<--List<--ArrayList

Collection<--List<--LinkedList

Collection<--Set<--HashSet

Collection<--Set<--HashSet<--LinkedHashSet

Collection<--Set<--SortedSet<--TreeSet

![史上最全java架构师技能图谱（上）][java 4]

**面向对象高级知识**

![史上最全java架构师技能图谱（上）][java 5]

类、对象、继承、构造函数、封装、接口、抽象类、多态、重写、this static关键字、类与对象的关系

**异常处理**

异常类类图：throwable exception error RuntimeException

异常处理机制

如何定义和使用异常

运行时异常和受检查异常区别

运行时错误

java异常处理的原则和技巧

**多线程**

![史上最全java架构师技能图谱（上）][java 6]

概念与原理

创建于启动

线程的生命周期及五种基本状态

线程交互

死锁

调度合并

调度让步

调度休眠

同步方法

同步块

同步与锁

线程池

阻塞队列

**IO/NIO**

同步阻塞 同步非阻塞 异步IO

**反射**

**序列化**

**泛型**

**网络编程**

**高级特性**

**JVM**

**运行时数据区**：方法区、虚拟机栈、本地方法栈、堆、程序计算器

**GC算法：**

内参回收三要素：什么内容需要回收、什么时候回收、如何回收

并发与执行

引用计数算法

根搜索算法

垃圾回收算法：标记-清楚算法 复制算法 标记-整理算法 分代手机算法

垃圾收集器：新生代、老年代收集器

**溢出**

java堆溢出

方法区溢出

outofmemoryerror

虚拟机栈和本地方法栈溢出

直接内容溢出

# **三：Web开发核心** #

![史上最全java架构师技能图谱（上）][java 7]

**HTML JS CSS**

html js css语法基础

Js css框架

Html开发工具

JS和CSS调试工具

**模板引擎**

jsp

velocity

freemarker

**Java web**

容器：tomcat jetty等

热部署插件：run-jetty-run

cookie session使用和区别

fliter和listener的启动和步骤

身份验证

单点登录原理以及实现

**web核心**

事物JTA

JMX

安全：JCCA/JAAS

通信:JNDI/JMS

SSI技术

**linux**

![史上最全java架构师技能图谱（上）][java 8]

常用命令以及操作系统原理等

**线上故障处理和分析**

![史上最全java架构师技能图谱（上）][java 9]

**性能工具** 

visualVM Jprofiler JMeter等

**线上故障**

线程数超标

访问超时

长事务

CPU超标

内存超标

**开发工具使用**

![史上最全java架构师技能图谱（上）][java 10]

**web开发调试**

firebug

Web Developer

JavaScript Debugger

IETester

Yslow

**构建工具**

maven Grails

maven私服 nexus

**版本控制**

git svn

**java调试工具**

JCover

Junit

Jtest

以及大量的eclipse插件，eg：findbugs等

# **开发框架** #

![史上最全java架构师技能图谱（上）][java 11]

SSH：struts2+spring+hibernate

SSM:springmvc+spring+mybatis

**阿里开源框架**

![史上最全java架构师技能图谱（上）][java 12]

如果你对架构师比较感兴趣，可以查看**优知学院****官网架构师进阶系列文章**。

以上内容就是java架构师技能图谱上篇，下篇将包含大数据以及性能、设计模式、UML、中间件、分布式集群、负载均衡、通讯协议、架构设计等技术图谱等章节。


[java]: /pro/os/crawler/NRVZ-EIA3-M2AA.gif
[java 1]: /pro/os/crawler/6BVY-UF6R-ZIYB.jpg
[java 2]: /pro/os/crawler/BQQY-A3RF-ZUBU.jpg
[java 3]: /pro/os/crawler/7VYY-IZYU-BMYV.jpg
[java 4]: /pro/os/crawler/7NQE-JZ26-RVNN.jpg
[java 5]: /pro/os/crawler/QNZJ-3Y3M-NAZ2.jpg
[java 6]: /pro/os/crawler/FUAN-E3FJ-QAMY.jpg
[java 7]: /pro/os/crawler/UFYB-RMZZ-ZE2E.jpg
[java 8]: /pro/os/crawler/EZER-AJNV-ZFVJ.jpg
[java 9]: /pro/os/crawler/NI3Q-YBR2-AVZ2.jpg
[java 10]: /pro/os/crawler/F3EJ-BJBR-2AQ2.jpg
[java 11]: /pro/os/crawler/VIZE-EFBF-MYYI.jpg
[java 12]: /pro/os/crawler/UQUJ-ZUQA-EBNM.jpg
 *  **原文作者：** 优知学院
 *  **原文链接：** https://www.toutiao.com/item/6463222759680901646/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。