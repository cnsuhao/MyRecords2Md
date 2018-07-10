---
title: Java定时任务调度详解
date: 2018-06-22 16:52:00
categories: "开发"
tags:
	- Java
	- 技术
	- Linux
	- TRIGGER
	- 编程语言

---

# 前言 #

在实际项目开发中，除了Web应用、SOA服务外，还有一类不可缺少的，那就是定时任务调度。定时任务的场景可以说非常广泛，比如某些视频网站，购买会员后，每天会给会员送成长值，每月会给会员送一些电影券；比如在保证最终一致性的场景中，往往利用定时任务调度进行一些比对工作；比如一些定时需要生成的报表、邮件；比如一些需要定时清理数据的任务等。本篇博客将系统的介绍定时任务调度，会涵盖Timer、ScheduledExecutorService、开源工具包Quartz，以及Spring和Quartz的结合等内容。

# JDK原生定时工具：Timer #

> **定时任务调度：基于给定的时间点、给定的时间间隔、给定的执行次数自动执行的任务。**
> 
> Timer位于java.util包下，其内部包含且仅包含一个后台线程（TimeThread）对多个业务任务（TimeTask）进行定时定频率的调度。

schedule的四种用法和scheduleAtFixedRate的两种用法

![Java定时任务调度详解][Java]

Timer的核心方法

> 参数说明：
> 
> **task**：所要执行的任务，需要extends TimeTask override run()
> 
> **time/firstTime**：首次执行任务的时间
> 
> **period**：周期性执行Task的时间间隔，单位是毫秒
> 
> **delay**：执行task任务前的延时时间，单位是毫秒
> 
> 很显然，通过上述的描述，我们可以实现：
> 
> **延迟多久后执行一次任务；指定时间执行一次任务；延迟一段时间，并周期性执行任务；指定时间，并周期性执行任务；**

**思考1：如果time/firstTime指定的时间，在当前时间之前，会发生什么呢？**

> **在时间等于或者超过time/firstTime的时候，会执行task！**也就是说，如果time/firstTime指定的时间在当前时间之前，就会立即得到执行。

**思考2：schedule和scheduleAtFixedRate有什么区别？**

> scheduleAtFixedRate：**每次执行时间为上一次任务开始起向后推一个period间隔**，也就是说下次执行时间相对于上一次任务开始的时间点，因此执行时间不会延后，但是存在任务并发执行的问题。
> 
> schedule：**每次执行时间为上一次任务结束后推一个period间隔**，也就是说下次执行时间相对于上一次任务结束的时间点，因此执行时间会不断延后。

**思考3：如果执行task发生异常，是否会影响其他task的定时调度？**

> 如果TimeTask抛出RuntimeException，那么Timer会停止所有任务的运行！

**思考4：Timer的一些缺陷？**

> 前面已经提及到Timer背后是一个单线程，因此Timer存在管理并发任务的缺陷：所有任务都是由同一个线程来调度，所有任务都是串行执行，意味着同一时间只能有一个任务得到执行，而前一个任务的延迟或者异常会影响到之后的任务。
> 
> 其次，Timer的一些调度方式还算比较简单，无法适应实际项目中任务定时调度的复杂度。

**一个简单的Demo实例**

![Java定时任务调度详解][Java 1]

Timer Demo

![Java定时任务调度详解][Java 2]

运行结果

**Timer其他需要关注的方法**

> **cancel()**：终止Timer计时器，丢弃所有当前已安排的任务（TimeTask也存在cancel()方法，不过终止的是TimeTask）
> 
> **purge()**：从计时器的任务队列中移除已取消的任务，并返回个数

# JDK对定时任务调度的线程池支持：ScheduledExecutorService #

> 由于Timer存在的问题，JDK5之后便提供了基于线程池的定时任务调度：ScheduledExecutorService。
> 
> **设计理念：每一个被调度的任务都会被线程池中的一个线程去执行，因此任务可以并发执行，而且相互之间不受影响。**

我们直接看例子：

![Java定时任务调度详解][Java 3]

基于线程池的定时任务调度

运行结果：

![Java定时任务调度详解][Java 4]

result

# 定时任务大哥：Quartz #

> 虽然ScheduledExecutorService对Timer进行了线程池的改进，但是依然无法满足复杂的定时任务调度场景。因此OpenSymphony提供了强大的开源任务调度框架：Quartz。Quartz是纯Java实现，而且作为Spring的默认调度框架，由于Quartz的强大的调度功能、灵活的使用方式、还具有分布式集群能力，可以说Quartz出马，可以搞定一切定时任务调度！

**Quartz的体系结构**

![Java定时任务调度详解][Java 5]

Quartz体系结构

先来看一个Demo：

![Java定时任务调度详解][Java 6]

业务Job

![Java定时任务调度详解][Java 7]

Quartz

> 说明：
> 
> **1、从代码上来看，有XxxBuilder、XxxFactory，说明Quartz用到了Builder、Factory模式，还有非常易懂的链式编程风格。**
> 
> **2、Quartz有3个核心概念：调度器（Scheduler）、任务（Job&JobDetail）、触发器（Trigger）。（一个任务可以被多个触发器触发，一个触发器只能触发一个任务）**
> 
> **3、注意当Scheduler调度Job时，实际上会通过反射newInstance一个新的Job实例（待调度完毕后销毁掉），同时会把JobExecutionContext传递给Job的execute方法，Job实例通过JobExecutionContext访问到Quartz运行时的环境以及Job本身的明细数据。**
> 
> **4、JobDataMap可以装载任何可以序列化的数据，存取很方便。需要注意的是JobDetail和Trigger都可以各自关联上JobDataMap。JobDataMap除了可以通过上述代码获取外，还可以在YourJob实现类中，添加相应setter方法获取。**
> 
> **5、Trigger用来告诉Quartz调度程序什么时候执行，常用的触发器有2种：SimpleTrigger（类似于Timer）、CronTrigger（类似于Linux的Crontab）。**
> 
> **6、实际上，Quartz在进行调度器初始化的时候，会加载quartz.properties文件进行一些属性的设置，比如Quartz后台线程池的属性（threadCount）、作业存储设置等。它会先从工程中找，如果找不到那么就是用quartz.jar中的默认的quartz.properties文件。**
> 
> **7、Quartz存在监听器的概念，比如任务执行前后、任务的添加等，可以方便实现任务的监控。**

**CronTrigger示例**

![Java定时任务调度详解][Java 8]

CronTrigger

**Cron表达式的写法**

![Java定时任务调度详解][Java 9]

特殊符号说明

![Java定时任务调度详解][Java 10]

cron表达式

> 这里给出一些常用的示例：
> 
> **0 15 10 \* \* ? \***每天10点15分触发
> 
> **0 15 10 \* \* ? 2017**2017年每天10点15分触发
> 
> **0 \* 14 \* \* ?**每天下午的 2点到2点59分每分触发
> 
> **0 0/5 14 \* \* ?**每天下午的 2点到2点59分(整点开始，每隔5分触发)
> 
> **0 0/5 14,18 \* \* ?**每天下午的 2点到2点59分、18点到18点59分(整点开始，每隔5分触发)
> 
> **0 0-5 14 \* \* ?**每天下午的 2点到2点05分每分触发
> 
> **0 15 10 ? \* 6L**每月最后一周的星期五的10点15分触发
> 
> **0 15 10 ? \* 6\#3**每月的第三周的星期五开始触发
> 
> **我们可以通过一些Cron在线工具非常方便的生成，比如http://www.pppet.net/等。**

# Spring和Quartz的整合 #

> 实际上，Quartz和Spring结合是很方便的，无非就是进行一些配置。大概基于2种方式：
> 
> 第一，普通的类，普通的方法，直接在配置中指定（**MethodInvokingJobDetailFactoryBean**）。
> 
> 第二，需要继承**QuartzJobBean，复写指定方法（executeInternal）即可。**
> 
> 然后，就是一些触发器、调度器的配置了，这里不再展开介绍了，只要弄懂了原生的Quartz的使用，那么和Spring的结合使用就会很简单。

**好了，到这里，就结束了，周末愉快！**


[Java]: /pro/os/crawler/IMVB-BMJ2-UJR2.jpg
[Java 1]: /pro/os/crawler/ZF6F-MNZI-67F3.jpg
[Java 2]: /pro/os/crawler/YFBM-2EVR-UVUB.jpg
[Java 3]: /pro/os/crawler/VRMB-2QMM-VM2A.jpg
[Java 4]: /pro/os/crawler/RIAV-BUI7-BBAB.jpg
[Java 5]: /pro/os/crawler/F3EQ-3QRZ-VFN3.jpg
[Java 6]: /pro/os/crawler/2EEM-NJFB-ZQFJ.jpg
[Java 7]: /pro/os/crawler/NREF-IUBV-FEMN.jpg
[Java 8]: /pro/os/crawler/RMVZ-3URY-ZRFR.jpg
[Java 9]: /pro/os/crawler/E2IY-IMMA-RNM2.jpg
[Java 10]: /pro/os/crawler/73MF-BVIR-M2AZ.jpg
 *  **原文作者：** 互联网Java架构
 *  **原文链接：** https://www.toutiao.com/item/6569829022350443015/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。