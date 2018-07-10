---
title: 自己搭建一个联网题库系统(java SERVER)
date: 2018-06-03 22:23:47
categories: "开发"
tags:
	- Java
	- 软件
	- XML
	- 软件开发过程
	- 编程语言

---

# 导语 #

在线题库系统是很多企业及学校必备的一个软件，它兼具考核，评估，诊断等多种功能，方便了企业和高校对于学生或员工的考核管理，因此，在线题库系统在如今已经占有一个举足轻重的地位，所以，做好题库系统不仅决定了工作的本身，更是公司企业能否成功的最重要的一环。我们必须有能力，有必要完成好这项任务，下面，我们开始一步一步搭建在线题库系统

# 架构选择 #

**架构的比较**

两种选择

> SSH 通常指的是 Struts2 做控制器(controller)，spring 管理各层的组件，**hibernate** 负责持久化层。
> 
> SSM 则指的是 SpringMVC 做控制器(controller)，Spring 管理各层的组件，MyBatis 负责持久化层。

![自己搭建一个联网题库系统(java SERVER)][java SERVER]

Struts2 的实现原理

**针对于控制器的选择**

1.Struct和Spring-MVC都是负责取转发的，但是两者针对request的请求上面区别很大，Struct是针对一个Action类来进行请求的，即一个Action类对应于一个请求，所以对应得失类拦截器，请求的数据类共享。而Spring-MVC则是针对于方法级别的请求的，也就是一个方法对应于一个请求，属于方法拦截，请求的数据方法不共享。

2.Spring-MVC的配置文件相对来说较容易上手，可以提高软件开发的效率。

3.Spring-MVC是基于Servlet级别的而Struct的级别是Filter级别的。

> struts拦截器
> 
> 1. Struts2拦截器是在访问某个Action或Action的某个方法，字段之前或之后实施拦截，并且Struts2拦截器是可插拔的，拦截器是AOP的一种实现，而Filter是责任链的一种实现．
> 
> 2. 拦截器栈（Interceptor Stack）。Struts2拦截器栈就是将拦截器按一定的顺序联结成一条链。在访问被拦截的方法或字段时，Struts2拦截器链中的拦截器就会按其之前定义的顺序被调用。
> 
> **二、Struts2 拦截器接口实现：**
> 
> Struts2规定用户自定义拦截器必须实现com.opensymphony.xwork2.interceptor.Interceptor接口。该接口声明了3个方法，其中，init和destroy方法会在程序开始和结束时各执行一遍，不管使用了该拦截器与否，只要在struts.xml中声明了该Struts2拦截器就会被执行。intercept方法就是拦截的主体了，每次拦截器生效时都会执行其中的逻辑。

说到这里，可能有的人会不懂，拦截器和过滤器有什么区别？

**（1）过滤器：**

依赖于servlet容器。在实现上基于函数回调，可以对几乎所有请求进行过滤，但是缺点是一个过滤器实例只能在容器初始化时调用一次。使用过滤器的目的是用来做一些过滤操作，获取我们想要获取的数据，比如：在过滤器中修改字符编码；在过滤器中修改HttpServletRequest的一些参数，包括：过滤低俗文字、危险字符等

**（2）拦截器：**

依赖于web框架，在struts中就是依赖于依赖框架。在实现上基于Java的反射机制，属于面向切面编程（AOP）的一种运用。由于拦截器是基于web框架的调用，因此可以使用Spring的依赖注入（DI）进行一些业务操作，同时一个拦截器实例在一个controller生命周期之内可以多次调用。但是缺点是只能对controller请求进行拦截，对其他的一些比如直接访问静态资源的请求则没办法进行拦截处理

ssh框架的好处

> 1. 典型的三层构架体现MVC实现，可以让开发人员减轻重新建立解决复杂问题方案的负担和精力。便于敏捷开发出新的需求，降低开发时间成本。
> 
> 2. 良好的可扩展性，ssh主流技术有强大的用户社区支持它，所以该框架扩展性非常强，针对特殊应用时具有良好的可插拔性，避免大部分因技术问题不能实现的功能。
> 
> 3. 良好的可维护性，业务系统经常会有新需求，三层构架因为逻辑层和展现层的合理分离，可使需求修改的风险降低到最低。随着新技术的流行或系统的老化，系统可能需要重构，ssh构架重构成功率要比其他构架高很多。
> 
> 4. 优秀的解耦性，提高了内聚，降低了耦合，很少有软件产品的需求从一开始就完全是固定的。客户对软件需求，是随着软件开发过程的深入，不断明晰起来的。因此，常常遇到软件开发到一定程度时，由于客户对软件需求发生了变化，使得软件的实现不得不随之改变。ssh三层构架，控制层依赖于业务逻辑层，但绝不与任何具体的业务逻辑组件耦合，只与接口耦合；同样，业务逻辑层依赖于DAO层，也不会与任何具体的DAO组件耦合，而是面向接口编程。采用这种方式的软件实现，即使软件的部分发生改变，其他部分也不会改变。

所以我们选择ssh框架

# 搭建ssh框架 #

struts流程图

![自己搭建一个联网题库系统(java SERVER)][java SERVER 1]

请求流程图

2、spring的流程图：　　

![自己搭建一个联网题库系统(java SERVER)][java SERVER 2]

　解析：上图是在struts结构图的基础上加入了spring流程图，在web.xml配置文件中加入了spring的监听器，在struts.xml配置文件中添加“<constant name="struts.objectFactory" value="spring" />”是告知Struts2运行时使用Spring来创建对象，spring在其中主要做的就是注入实例，将所有需要类的实例都由spring管理。

2、hibernate的核心架构和执行流程图：

![自己搭建一个联网题库系统(java SERVER)][java SERVER 3]

![自己搭建一个联网题库系统(java SERVER)][java SERVER 4]

# **搭建一个完整的SSH框架项目** #

 **(1) 基于配置文件的整合：**

 **我们需要在web.xml中定义一个Struts2的filter：**

 **第三步：在web.xml中配置一个监听器，因为如果我们要加载applicationContext.xml，然而action是多实例的，如果每请求一次action就要加载一次xml的话，这会使得你的整个项目效率十分低 下，因此，我的想法是把applicationContext.xml文件放在servletContext中，只加载一次，因此我们就需要配置一个servletContext的监听器。**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 5]

 **第四步：先开始Struts2与Spring的整合：先把service，dao，entity，action这些层次建好**

 **Struts2自己管理action的方式：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 6]

 **action交给Spring管理：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 7]

**action交给Spring管理的话，Struts.xml文件里action的class不应该写全类名，只需要写Spring里的id名即可，并且 要设置scope="prototype"，因为action是多例的！！**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 8]

**传统方式要通过类来getBean获取service，而这里只需要配置一下常量就可以在action里不需要通过注解或者配置 文件就可以对service直接进行调用，只需要设置一下setService方法即可！！**

 **第五步：Spring与Hibernate的整合：**

 **有两种方式：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 9]

**二、要配置事务管理哦！！**

 **之所以直接注入sessionFactory就可以调用模板，你打开HibernateDaoSupport类，可以发现，在里面有个setSessionFactory方法里，创建了template** 

![自己搭建一个联网题库系统(java SERVER)][java SERVER 10]

 **同时要再业务层（service层，用以控制事务）加上：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 11]

 **（2）无Hibernate配置文件形式**

**在Spring中就要配置好Hibernate的一些属性：**

 **1.c3p0连接池的信息：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 12]

 **2.hibernate常用属性：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 13]

 **3.映射关系：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 14]

 **关于在dao层查询出来的result，如果要传到web层的话，可能在service层就已经将事务关闭，因此在web层不能及时的接收到要获取的对象，因此我们要在web层开启事务！！**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 15]

 **(2) 基于注解的整合：**

 **第1-3步：前三步和上面的xml形式整合是一样的，就不细说了！**

 **第四步：创建一个处理请求的Action**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 16]

 **第五步：注册处理请求实体：**

![自己搭建一个联网题库系统(java SERVER)][java SERVER 17]

 **第六步：之后就是事务管理器的注册和模板注入。**


[java SERVER]: /pro/os/crawler/Z7VE-IVFF-B3EE.jpg
[java SERVER 1]: /pro/os/crawler/YN2U-Q2BQ-RFQJ.jpg
[java SERVER 2]: /pro/os/crawler/63QA-Q3V2-U3AV.jpg
[java SERVER 3]: /pro/os/crawler/6JUR-UBVV-A3YN.jpg
[java SERVER 4]: /pro/os/crawler/E7VZ-NAQQ-Y7FA.jpg
[java SERVER 5]: /pro/os/crawler/EYQY-MVJ7-JB7R.jpg
[java SERVER 6]: /pro/os/crawler/A3YN-QRJQ-6JZ3.jpg
[java SERVER 7]: /pro/os/crawler/A7BJ-2QVA-MYYF.jpg
[java SERVER 8]: /pro/os/crawler/BFAU-MUFV-AFZB.jpg
[java SERVER 9]: /pro/os/crawler/M263-MJFE-UZZM.jpg
[java SERVER 10]: /pro/os/crawler/ZIQU-QMIQ-2UVB.jpg
[java SERVER 11]: /pro/os/crawler/AM2E-QBJU-ZMRB.jpg
[java SERVER 12]: /pro/os/crawler/FNIA-UUQ6-7V6R.jpg
[java SERVER 13]: /pro/os/crawler/ZUJ7-FZ7R-IYZQ.jpg
[java SERVER 14]: /pro/os/crawler/MNEA-BANZ-VYZ3.jpg
[java SERVER 15]: /pro/os/crawler/JQNJ-EJVQ-2QAF.jpg
[java SERVER 16]: /pro/os/crawler/YFRZ-MBZR-NBQV.jpg
[java SERVER 17]: /pro/os/crawler/RYMI-7FZF-YME3.jpg
 *  **原文作者：** 青峰科技
 *  **原文链接：** https://www.toutiao.com/item/6562863908124623363/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。