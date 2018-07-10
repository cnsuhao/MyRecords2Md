---
title: 好纲要用在刀刃上，适时选择不同的线程池来实现
date: 2018-04-07 00:15:00
categories: "开发"
tags:
	- Java
	- 技术
	- 新创建集团
	- 编程语言

---

Java的线程池实现从根本上来说只有两个：ThreadPoolExecutor类和ScheduledThreadPoolExecutor类，这两个类还是父子关系，但是Java为了简化并行计算，还提供了一个Exceutors的静态类，它可以直接生成多种不同的线程池执行器，比如单线程执行器、带缓冲功能的执行器等，但归根结底还是使用ThreadPoolExecutor类或ScheduledThreadPoolExecutor类的封装类。

为了理解这些执行器，我们首先来看看ThreadPoolExecutor类，其中它复杂的构造函数可以很好的理解线程池的作用，代码如下：

![好纲要用在刀刃上，适时选择不同的线程池来实现][152300236012837e1a3be72]

这是ThreadPoolExecutor最完整的构造函数，其他的构造函数都是引用该构造函数实现的，我们逐步来解释这些参数的含义。

1.  **corePoolSize：最小线程数**。线程启动后，在池中保持线程的最小数量。需要说明的是线程数量是逐步到达corePoolSize值的，例如corePoolSize被设置为10，而任务数量为5，则线程池中最多会启动5个线程，而不是一次性的启动10个线程。
2.  **maximumPoolSize：最大线程数量**。这是池中最大能容纳的最大线程数量，如果超出，则使用RejectedExecutionHandler 拒绝策略处理。
3.  **keepAliveTime：线程最大生命周期**。这里的生命周期有两个约束条件，一是该参数针对的是超过corePoolSize数量的线程。二是处于非运行状态的线程。这么说吧，如果corePoolSize为10，maximumPoolSize为20，此时线程池中有15个线程正在运行，一段时间后，其中有3个线程处于等待状态的时间超过了keepAliveTime指定的时间，则结束这3个线程，此时线程池中还有12个线程正在运行。
4.  **unit：时间单位。**这是keepAliveTime的时间单位，可以是纳秒、毫秒、秒、分等选项。
5.  **workQuene：任务队列**。当线程池中的线程都处于运行状态，而此时任务数量继续增加，则需要一个容器来容纳这些任务，这就是任务队列。
6.  **threadFactory：线程工厂。**定义如何启动一个线程，可以设置线程名称，并且可以确认是否是后台线程等。
7.  **handler：拒绝任务处理器。**由于超出线程数量和队列容量而对继续增加的任务进行处理的程序。

[Java并发编程从入门到精通

￥31.2

购买][Java_ _31.2]

线程池的管理是这样一个过程：首先创建线程池，然后根据任务的数量逐步将线程增大到corePoolSize数量，如果此时仍有任务增加，则放置到workQuene中，直到workQuene爆满为止，然后继续增加池中的数量(增强处理能力)，最终达到maximumPoolSize，那如果此时还有任务增加进来呢？这就需要handler处理了，或者丢弃任务，或者拒绝新任务，或者挤占已有任务等。

在任务队列和线程池都饱和的情况下，一但有线程处于等待(任务处理完毕，没有新任务增加)状态的时间超过keepAliveTime，则该线程终止，也就说池中的线程数量会逐渐降低，直至为corePoolSize数量为止。

我们可以把线程池想象为这样一个场景：在一个生产线上，车间规定是可以有corePoolSize数量的工人，但是生产线刚建立时，工作不多，不需要那么多的人。随着工作数量的增加，工人数量也逐渐增加，直至增加到corePoolSize数量为止。此时还有任务增加怎么办呢？

好办，任务排队，corePoolSize数量的工人不停歇的处理任务，新增加的任务按照一定的规则存放在仓库中(也就是我们的workQuene中)，一旦任务增加的速度超过了工人处理的能力，也就是说仓库爆满时，车间就会继续招聘工人(也就是扩大线程数)，直至工人数量到达maximumPoolSize为止，那如果所有的maximumPoolSize工人都在处理任务时，而且仓库也是饱和状态，新增任务该怎么处理呢？这就会扔一个叫handler的专门机构去处理了，它要么丢弃这些新增的任务，要么无视，要么替换掉别的任务。

过了一段时间后，任务的数量逐渐减少，导致一部分工人处于待工状态，为了减少开支(Java是为了减少系统的资源消耗)，于是开始辞退工人，直至保持corePoolSize数量的工人为止，此时即使没有工作，也不再辞退工人(池中的线程数量不再减少)，这也是保证以后再有任务时能够快速的处理。

明白了线程池的概念，我们再来看看Executors提供的几个线程创建线程池的便捷方法：

 *  newSingleThreadExecutor：单线程池。顾名思义就是一个池中只有一个线程在运行，该线程永不超时，而且由于是一个线程，当有多个任务需要处理时，会将它们放置到一个无界阻塞队列中逐个处理，它的实现代码如下：

![好纲要用在刀刃上，适时选择不同的线程池来实现][152300247567031b8122aa6]

它的使用方法也很简单，下面是简单的示例：

![好纲要用在刀刃上，适时选择不同的线程池来实现][15230024989817664004193]

 *  newCachedThreadPool：缓冲功能的线程。建立了一个线程池，而且线程数量是没有限制的(当然，不能超过Integer的最大值)，新增一个任务即有一个线程处理，或者复用之前空闲的线程，或者重亲启动一个线程，但是一旦一个线程在60秒内一直处于等待状态时（也就是一分钟无事可做），则会被终止，其源码如下：

![好纲要用在刀刃上，适时选择不同的线程池来实现][ZAQZ-YJMM-RUBM.jpg]

这里需要说明的是，任务队列使用了同步阻塞队列，这意味着向队列中加入一个元素，即可唤醒一个线程(新创建的线程或复用空闲线程来处理)，这种队列已经没有队列深度的概念了.

 *  newFixedThreadPool：固定线程数量的线程池。 在初始化时已经决定了线程的最大数量，若任务添加的能力超出了线程的处理能力，则建立阻塞队列容纳多余的任务，其源码如下：

![好纲要用在刀刃上，适时选择不同的线程池来实现][1523002558513fcc7f4f724]

上面返回的是一个ThreadPoolExecutor，它的corePoolSize和maximumPoolSize是相等的，也就是说，最大线程数量为nThreads。如果任务增长的速度非常快，超过了LinkedBlockingQuene的最大容量(Integer的最大值)，那此时会如何处理呢？会按照ThreadPoolExecutor默认的拒绝策略(默认是DiscardPolicy，直接丢弃)来处理。

以上三种线程池执行器都是ThreadPoolExecutor的简化版，目的是帮助开发人员屏蔽过得线程细节，简化多线程开发。当需要运行异步任务时，可以直接通过Executors获得一个线程池，然后运行任务，不需要关注ThreadPoolExecutor的一系列参数是什么含义。当然，有时候这三个线程不能满足要求，此时则可以直接操作ThreadPoolExecutor来实现复杂的多线程计算。可以这样比喻，newSingleThreadExecutor、newCachedThreadPool、newFixedThreadPool是线程池的简化版，而ThreadPoolExecutor则是旗舰版\_\_\_简化版容易操作，需要了解的知识相对少些，方便使用，而旗舰版功能齐全，适用面广，难以驾驭。

**有讨论，才有进步，大家各抒己见，让每位同学学到不一样的！**


[152300236012837e1a3be72]: http://p1.pstatp.com/large/pgc-image/152300236012837e1a3be72
[Java_ _31.2]: http://union-click.jd.com/jdc?e=&amp;p=AyIHZRtYFAcXBFIZWR0yEgdTGV0RARY3EUQDS10iXhBeGlcJDBkNXg9JHUlSSkkFSRwSB1MZXREBFhgMXgdIMk1xV2csSn56ZB5bWFBjUUQgGxIPcEQLWXVYFAIbG1QeRxQFAwdSEFgVCRIFZRleFAsaAVUeWCUAEANVGVwVCxY3ZRtaJVB83%2BOtg7CzDtP%2FlI6dlSIGZRtfFgATBlYSXRcFEA5lHGtOWk1EDV4PSVJKaQdFByUyIjdlK1slAQ%3D%3D&amp;t=W1dCFFlQCxxKQgFHRE5XDVULR0UVAhQFUx9YER1LQglG
[152300247567031b8122aa6]: http://p1.pstatp.com/large/pgc-image/152300247567031b8122aa6
[15230024989817664004193]: http://p3.pstatp.com/large/pgc-image/15230024989817664004193
[ZAQZ-YJMM-RUBM.jpg]: /pro/os/crawler/ZAQZ-YJMM-RUBM.jpg
[1523002558513fcc7f4f724]: http://p1.pstatp.com/large/pgc-image/1523002558513fcc7f4f724
 *  **原文作者：** Free码农
 *  **原文链接：** https://www.toutiao.com/item/6541247964742943236/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。