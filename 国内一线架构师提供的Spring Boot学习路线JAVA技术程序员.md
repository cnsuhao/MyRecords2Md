---
title: 国内一线架构师提供的Spring Boot学习路线
date: 2018-06-26 16:24:58
categories: "开发"
tags:
	- Java
	- 软件
	- XML
	- 编程语言
	- Gradle

---

Spring Boot 学习路线，本文计划根据作者多年经验，来分析和制定一个学习使用 Spring Boot技术的步骤路线图。

**一、准备工作**

俗话说：“工欲善其事必先利其器”。特别是软件开发，这样一个重视工程实践的领域，一定要最先选择和熟悉一系列的开发环境工具。

首先推荐选用最新版本技术标准的开发工具，将如下的开发环境安装配置好。

开发环境：

(1)JDK 1.8

(2)Eclipse Oxygen EE版本 或者也可以使用 IntelliJ IDEA

(3)Tomcat 9

安装配置好了，如上的开发工具后，可以在环境下，去新建普通的Java project、Dynamic web project，做一个“Hello World”式的入门程序，去验证环境可以正常使用。

然后，就可以进入到 Spring Boot 的技术学习了!

**二、Spring Boot 知识入门**

对计算机技术知识的学习和使用，我建议理论联系实际。在很多时候，我们接触到一个新的技术的时候，最开始，肯定是被这些技术涉及到的术语、词汇所困扰，不明白这些技术术语词汇的定义、概念、含义，没有这些做根基，就很难做到掌握和学习这个技术，并达到融汇贯通的程度。

所以，学习 Spring Boot ，首先就要从宏观的层面上，去了解这个技术它的背景知识、运用场景、发展渊源，演进历史，这一块，可以通过在网络上搜索到大量的知识介绍。

建议访问spring官网：**https://spring.io/** 去获取最权威的介绍和定义。

![国内一线架构师提供的Spring Boot学习路线][Spring Boot]

我这里，强调一点：Spring Boot 运用“约定优于配置”的思想，对使用 Spring Boot创建的工程，提供约定、缺省、默认的配置，去简化传统手动、一步一步配置的开发方式；

![国内一线架构师提供的Spring Boot学习路线][Spring Boot 1]

**三、Spring 技术基础知识**

这一部分技术知识，已经是 Java EE开发必备的知识。包括IOC 和 AOP，重点要把IOC弄清楚，其次再说AOP。

![国内一线架构师提供的Spring Boot学习路线][Spring Boot 2]

**1、Bean工厂**

Spring中，存在一个 Bean工厂。我们把每一个java类当做是一个 bean（即豌豆），Spring就可以当做是一个factory（工厂），bean factory（豌豆工厂）的功能就是专门生产bean的。也就是说：Spring 可以去生产类的对象，也即 实例化类对象（new 类名();）。

**2、IOC/DI 控制反转/依赖注入**

从新建一个Dynamic Web project开始，在工程项目中，引入 最新版本的 Spring jar包，配置使用Spring，熟悉Spring框架在项目中，所起到的作用。重点学习了解IOC/DI，即“控制反转、依赖注入”

简单的说清楚，IOC产生的原因，及解决的问题。

在“三层结构”(表示层、业务逻辑层、数据访问层)架构开发中，层与层之间，类有调用依赖的关系

表示层——>业务逻辑层——>数据访问层，表示层类中，需要调用业务逻辑层类的方法；业务逻辑层类中，需要调用数据访问层类的方法。

以用户登录、注册、用户个人资料维护为例：设计3个类 UserController， UserService， UserDao ，分别对应“三层结构”的表示层、业务逻辑层、数据访问层。

那么，在项目实际开发代码中，存在：

表示层类 UserController 中，要引用 UserService：

``````````
public Class UserController { UserService userService = new UserService(); ……}
``````````

业务逻辑层类 UserService 中，要引用 UserDao：

``````````
public Class UserService { UserDao userDao = new UserDao(); ……}
``````````

我们会发现 类与类之间，有很紧密的依赖关系，即：在代码里，一个类中，引用了另外一个类，并 new 了一个对象。

这样，也就意味着有很强的耦合性。而这样，是不建议的。

因为软件设计中，很强调的是设计出来的软件需要具有很好的“特性：“高内聚、松耦合”。

为了解耦，降低这种很强的依赖性，Spring 框架中，设计出了 IOC。

代码中，不去 维护类与类之间的依赖性，也即： UserController 中不去new UserService()

而是去这样写：

``````````
public Class UserController { //UserService userService = new UserService(); UserService userService; ……}
``````````

那么 实例化UserService，即 new UserService();在哪里操作？

交给Spring 的bean工厂，去实例化。

如何去实例化类对象，有哪些需要约定，这就需要一个 xml配置文件去记录。后来，可以使用annotation（注解）的方式去配置属性。

spring配置文件：

``````````
<bean id="cat" class="test.spring.Cat" scope="prototype"> <property name="name" value="波斯猫"></property> </bean>
``````````

**3、AOP**

面向切面编程，主要解决横切性的问题。

什么是横切性的问题？比如开发中，有很多的类、很多的方法，类与类之间存在调用的依赖的关系，我们称之为“从上而下”的线性调用。在这些代码中，经常需要在很多位置，添加“打印日志”的代码。而这些，“打印日志”的代码，基本都是一样的，和“从上而下”的线性调用，没有什么直接的业务逻辑关系。我们可以称之为：横切到这个“从上而下”的线性中。就像一个“十字”、“垂直”、“正交”这样。

除了“日志”属于横切性问题，“事务”也属于。

AOP就是为了解决这种横切性的问题，通过配置，不让这些相同的代码，充斥在项目代码的各处。而是通过，很少的配置，把这些相似的横切性代码，配置到它们应该出现的位置。

AOP也需要了解一些，专门的术语，我们这里只是简单的介绍一下，AOP需要说清楚，还要写专门的文章，去举例和描述。

**四、Maven、Gradle**

简单的说：Maven 和 Gradle 都是解决相同的问题，就是我们在创建Java相关的项目工程时，项目中经常需要使用各种 框架或类库的.jar 包。传统入门的方式，是去专门的官网，去下载好这些jar包，复制粘贴到项目中，然后“Add to buildpath”。这样会造成一些问题。

（1）一些框架的 jar包有很多个，在项目中使用时，它们jar包之间存在依赖关系；

（2）而且随着时间的迁移，会不断的推出新的版本，新旧版本间可能会有冲突问题。

（3）而且一个项目，可能会使用好几个框架，这些框架中，都使用了一些相同的jar包，版本之间如何统一。

（4）同一台电脑中，创建多个工程，每个工程都使用了相同的框架，传统方式的结果就是，这些框架的jar包在电脑中，复制很多次。

……

Maven 和 Gradle 的出现就是，去维护和管理这些jar包。使得，只用去写一个配置文件，就可以自动的去使用这些jar包。

maven配置文件 pom.xml

``````````
<!-- spring begin --> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-webmvc</artifactId> <version>${spring.version}</version> </dependency>
``````````

**五、Spring Boot 的 Hello World程序**

在Eclipse中，创建一个 使用 Spring Boot 技术的 Hello World程序。去初步的体验，和接触Spring Boot ，有个直观的感受和印象。有助于后面慢慢的去深入了解和学习掌握这个技术。

![国内一线架构师提供的Spring Boot学习路线][Spring Boot 3]

**那如何学习才能快速入门并精通呢？**

当真正开始学习的时候难免不知道从哪入手，导致效率低下影响继续学习的信心。

但最重要的是不知道哪些技术需要重点掌握，学习时频繁踩坑，最终浪费大量时间，所以有一套实用的视频课程用来跟着学习是非常有必要的。

为了让学习变得轻松、高效，今天给大家免费分享一套阿里架构师传授的一套教学资源。帮助大家在成为架构师的道路上披荆斩棘。

这套视频课程详细讲解了（Spring，MyBatis，Netty源码分析，高并发、高性能、分布式、微服务架构的原理，JVM性能优化、分布式架构）等这些成为架构师必备的内容！

而且还把框架需要用到的各种程序进行了打包，根据基础视频可以让你轻松搭建分布式框架环境，像在企业生产环境一样进行学习和实践。

![国内一线架构师提供的Spring Boot学习路线][Spring Boot 4]

**后台私信回复 “ 架构 ” 就可以马上免费获得这套价值一万八的内部教材！**


[Spring Boot]: /pro/os/crawler/RJB7-VZEI-M6ZA.jpg
[Spring Boot 1]: /pro/os/crawler/JMEZ-BYVF-3IYQ.jpg
[Spring Boot 2]: /pro/os/crawler/AV36-JZME-EEUJ.jpg
[Spring Boot 3]: /pro/os/crawler/JEQB-F2RF-NQRY.jpg
[Spring Boot 4]: /pro/os/crawler/ZAIE-7VZV-IYNA.jpg
 *  **原文作者：** JAVA技术程序员
 *  **原文链接：** https://www.toutiao.com/item/6571306396619375111/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。