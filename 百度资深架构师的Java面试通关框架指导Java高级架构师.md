---
title: 百度资深架构师的Java面试通关框架指导
date: 2018-04-04 16:46:07
categories: "开发"
tags:
	- Java
	- 软件
	- XML
	- 编程语言
	- 网游

---

# Spring #

**BeanFactory 和 ApplicationContext 有什么区别**

BeanFactory 可以理解为含有bean集合的工厂类。BeanFactory 包含了种bean的定义，以便在接收到客户端请求时将对应的bean实例化。

BeanFactory还能在实例化对象的时生成协作类之间的关系。此举将bean自身与bean客户端的配置中解放出来。BeanFactory还包含了bean生命周期的控制，调用客户端的初始化方法（initialization methods）和销毁方法（destruction methods）。

从表面上看，application context如同bean factory一样具有bean定义、bean关联关系的设置，根据请求分发bean的功能。但application context在此基础上还提供了其他的功能。

提供了支持国际化的文本消息

统一的资源文件读取方式

已在监听器中注册的bean的事件

**Spring Bean 的生命周期**

Spring Bean的生命周期简单易懂。在一个bean实例被初始化时，需要执行一系列的初始化操作以达到可用的状态。同样的，当一个bean不在被调用时需要进行相关的析构操作，并从bean容器中移除。

Spring bean factory 负责管理在spring容器中被创建的bean的生命周期。Bean的生命周期由两组回调（call back）方法组成。

初始化之后调用的回调方法。

销毁之前调用的回调方法。

Spring框架提供了以下四种方式来管理bean的生命周期事件：

InitializingBean和DisposableBean回调接口

针对特殊行为的其他Aware接口

Bean配置文件中的Custom init()方法和destroy()方法

@PostConstruct和@PreDestroy注解方式

**Spring IOC 如何实现**

Spring中的 org.springframework.beans 包和 org.springframework.context包构成了Spring框架IoC容器的基础。

BeanFactory 接口提供了一个先进的配置机制，使得任何类型的对象的配置成为可能。ApplicationContex接口对BeanFactory（是一个子接口）进行了扩展，在BeanFactory的基础上添加了其他功能，比如与Spring的AOP更容易集成，也提供了处理message resource的机制（用于国际化）、事件传播以及应用层的特别配置，比如针对Web应用的WebApplicationContext。

org.springframework.beans.factory.BeanFactory 是Spring IoC容器的具体实现，用来包装和管理前面提到的各种bean。BeanFactory接口是Spring IoC 容器的核心接口

**Spring AOP 实现原理**

面向切面编程，在我们的应用中，经常需要做一些事情，但是这些事情与核心业务无关，比如，要记录所有update方法的执行时间时间，操作人等等信息，记录到日志，

通过spring的AOP技术，就可以在不修改update的代码的情况下完成该需求。

Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理。JDK动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK动态代理的核心是InvocationHandler接口和Proxy类。

如果目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。


**动态代理（cglib 与 JDK）**

JDK 动态代理类和委托类需要都实现同一个接口。也就是说只有实现了某个接口的类可以使用Java动态代理机制。但是，事实上使用中并不是遇到的所有类都会给你实现一个接口。因此，对于没有实现接口的类，就不能使用该机制。而CGLIB则可以实现对类的动态代理。如果想学习Java工程化、高性能及分布式、深入浅出。微服务、Spring，MyBatis，Netty源码分析的朋友可以加Java进阶交流群：582505643，群里有阿里大牛直播讲解技术，以及Java大型互联网技术的视频免费分享给大家。


**Spring 事务实现方式及底层原理**

1、编码方式

所谓编程式事务指的是通过编码方式实现事务，即类似于JDBC编程实现事务管理。

2、声明式事务管理方式

声明式事务管理又有两种实现方式：基于xml配置文件的方式；另一个实在业务方法上进行@Transaction注解，将事务规则应用到业务逻辑中

a、划分处理单元——IOC

由于spring解决的问题是对单个数据库进行局部事务处理的，具体的实现首相用spring中的IOC划分了事务处理单元。并且将对事务的各种配置放到了ioc容器中（设置事务管理器，设置事务的传播特性及隔离机制）。

b、AOP拦截需要进行事务处理的类

Spring事务处理模块是通过AOP功能来实现声明式事务处理的，具体操作（比如事务实行的配置和读取，事务对象的抽象），用TransactionProxyFactoryBean接口来使用AOP功能，生成proxy代理对象，通过TransactionInterceptor完成对代理方法的拦截，将事务处理的功能编织到拦截的方法中。读取ioc容器事务配置属性，转化为spring事务处理需要的内部数据结构（TransactionAttributeSourceAdvisor），转化为TransactionAttribute表示的数据对象。

c、对事物处理实现（事务的生成、提交、回滚、挂起）

spring委托给具体的事务处理器实现。实现了一个抽象和适配。适配的具体事务处理器：DataSource数据源支持、hibernate数据源事务处理支持、JDO数据源事务处理支持，JPA、JTA数据源事务处理支持。这些支持都是通过设计PlatformTransactionManager、AbstractPlatforTransaction一系列事务处理的支持。 为常用数据源支持提供了一系列的TransactionManager。

d、结合

PlatformTransactionManager实现了TransactionInterception接口，让其与TransactionProxyFactoryBean结合起来，形成一个Spring声明式事务处理的设计体系。

Spring MVC 的启动与运行流程

在 web.xml 文件中给 Spring MVC 的 Servlet 配置了 load-on-startup,所以程序启动的

时候会初始化 Spring MVC，在 HttpServletBean 中将配置的 contextConfigLocation

属性设置到 Servlet 中，然后在 FrameworkServlet 中创建了 WebApplicationContext,

DispatcherServlet 根据 contextConfigLocation 配置的 classpath 下的 xml 文件初始化了

Spring MVC 总的组件。

1.spring mvc将所有的请求都提交给DispatcherServlet,它会委托应用系统的其他模块负责对请求 进行真正的处理工作。

2.DispatcherServlet查询一个或多个HandlerMapping,找到处理请求的Controller.

3.DispatcherServlet请请求提交到目标Controller

4.Controller进行业务逻辑处理后，会返回一个ModelAndView

5.Dispathcher查询一个或多个ViewResolver视图解析器,找到ModelAndView对象指定的视图对象

6.视图对象负责渲染返回给客户端。

![百度资深架构师的Java面试通关框架指导][Java]

# Netty #

**为什么选择 Netty**

1) API使用简单，开发门槛低；

2) 功能强大，预置了多种编解码功能，支持多种主流协议；

3) 定制能力强，可以通过 ChannelHandler 对通信框架进行灵活的扩展；

4) 性能高，通过与其它业界主流的NIO框架对比，Netty的综合性能最优；

5) 成熟、稳定，Netty修复了已经发现的所有JDK NIO BUG，业务开发人员不需要再为NIO的BUG而烦恼；

6) 社区活跃，版本迭代周期短，发现的BUG可以被及时修复，同时，更多的新功能会被加入；

7) 经历了大规模的商业应用考验，质量已经得到验证。在互联网、大数据、网络游戏、企业应用、电信软件等众多行业得到成功商用，证明了它可以完全满足不同行业的商业应用。

正是因为这些优点，Netty逐渐成为Java NIO编程的首选框架。

**说说业务中，Netty 的使用场景**

构建高性能、低时延的各种Java中间件，例如MQ、分布式服务框架、ESB消息总线等，Netty主要作为基础通信框架提供高性能、低时延的通信服务；

公有或者私有协议栈的基础通信框架，例如可以基于Netty构建异步、高性能的WebSocket协议栈；

各领域应用，例如大数据、游戏等，Netty作为高性能的通信框架用于内部各模块的数据分发、传输和汇总等，实现模块之间高性能通信。如果想学习Java工程化、高性能及分布式、深入浅出。微服务、Spring，MyBatis，Netty源码分析的朋友可以加Java进阶交流群：582505643，群里有阿里大牛直播讲解技术，以及Java大型互联网技术的视频免费分享给大家。

**原生的 NIO 在 JDK 1.7 版本存在 epoll bug**

它会导致Selector空轮询，最终导致CPU 100%。官方声称在JDK 1.6版本的update18修复了该问题，但是直到JDK 1.7版本该问题仍旧存在，只不过该BUG发生概率降低了一些而已，它并没有得到根本性解决。


**Netty 线程模型**


首先，Netty使用EventLoop来处理连接上的读写事件，而一个连接上的所有请求都保证在一个EventLoop中被处理，一个EventLoop中只有一个Thread，所以也就实现了一个连接上的所有事件只会在一个线程中被执行。一个EventLoopGroup包含多个EventLoop，可以把一个EventLoop当做是Reactor线程模型中的一个线程，而一个EventLoopGroup类似于一个ExecutorService

**Netty 的零拷贝**

“零拷贝”是指计算机操作的过程中，CPU不需要为数据在内存之间的拷贝消耗资源。而它通常是指计算机在网络上发送文件时，不需要将文件内容拷贝到用户空间（User Space）而直接在内核空间（Kernel Space）中传输到网络的方式。


**Netty 内部执行流程**

1.  Netty的接收和发送ByteBuffer采用DIRECT BUFFERS，使用堆外直接内存进行Socket读写，不需要进行字节缓冲区的二次拷贝。如果使用传统的堆内存（HEAP BUFFERS）进行Socket读写，JVM会将堆内存Buffer拷贝一份到直接内存中，然后才写入Socket中。相比于堆外直接内存，消息在发送过程中多了一次缓冲区的内存拷贝。
2.  Netty提供了组合Buffer对象，可以聚合多个ByteBuffer对象，用户可以像操作一个Buffer那样方便的对组合Buffer进行操作，避免了传统通过内存拷贝的方式将几个小Buffer合并成一个大的Buffer。
3.  Netty的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标Channel，避免了传统通过循环write方式导致的内存拷贝问题。
    
    摘抄

**Netty 重连实现**


1.心跳机制检测连接存活

2.启动时连接重试

3.运行中连接断开时重试

![百度资深架构师的Java面试通关框架指导][Java 1]

![百度资深架构师的Java面试通关框架指导][Java 2]


[Java]: /pro/os/crawler/YYRQ-FIJZ-2QQN.jpg
[Java 1]: /pro/os/crawler/E2YJ-MNMA-EVJE.jpg
[Java 2]: /pro/os/crawler/UBJA-VBIA-BIV2.jpg
 *  **原文作者：** Java高级架构师
 *  **原文链接：** https://www.toutiao.com/item/6540511780894933507/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。