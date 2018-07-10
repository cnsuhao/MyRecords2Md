---
title: 在Spring boot中，怎样借助Aop来简单的获取用户的主机信息
date: 2017-09-02 11:25:45
categories: "开发"
tags:
	- 科技
	- CSDN
	- Chrome
	- Kotlin

---

> **作者：feintkotlin（Kotlin学习网）**
> 
> **附言：**想要学习更多关于Kotlin的知识，可以通过以下途径进行**：feintkotlin的知乎专栏（Kotlin学习），feintkotlin的CSDN博客，Kotlin学习网。**

Aop，又叫面向切面编程。关于它的具体的理论，在这里就不赘述了。我个人觉得使用aop最大的好处就是，能够把许多分散的，却又有着相同模式的工作全部集中在一个地方进行处理。即减少了代码量，又方便以后的维护工作。

要想在spring boot中使用aop的相关功能，你需要导入以下这个依赖

> <dependency>
> 
> <groupId>org.springframework.boot</groupId>
> 
> <artifactId>spring-boot-starter-aop</artifactId>
> 
> </dependency>

# 第一步：创建UserHost类    #

**说明：**UserHost用来保存用户主机的相关信息，包括：ip地址、主机名、操作系统、浏览器等信息，具体代码见下方  


![在Spring boot中，怎样借助Aop来简单的获取用户的主机信息][Spring boot_Aop]

# 第二步：创建一个Controller #

**说明：**提供服务接口，用于测试程序的功能，代码是这样子的：  


![在Spring boot中，怎样借助Aop来简单的获取用户的主机信息][Spring boot_Aop 1]

注意：服务接口所对应的函数具有一个HttpServletRequest类型参数，我们接下来就是根据它来获取用户主机的相关信息。

# 第三步：创建HostAspect类 #

**说明：**在这个类中进行aop的相关操作。具体代码见下图所示  


![在Spring boot中，怎样借助Aop来简单的获取用户的主机信息][Spring boot_Aop 2]

整个过程可以分成三个步骤：

1.  匹配切入点  
    
2.  通知
3.  在连接点进行相应操作  
    

寻找切面的时候，需要使用到Pointcut表达式。比如本例中的这个表达式的意思是：**com.feint.kotlinaoptest.controller包下的带有RequestMapping注解的所有方法**。

Pointcut表达式又很多种类型：（关于这些表达式的理解如果有不正确的地方，欢迎在评论中指出）

1.  execution(可见性修饰符（可选） 返回值 类型声明(参数) 抛出异常（可选）)：匹配符合表达式模式的所有方法
2.  within(类型声明)：匹配所有符和表达式模式的类，及其中所有的方法。例如：**@Pointcut("within(com.feint.kotlinaoptest.controller..\*)")。**其匹配了controller包下面所有的类。
3.  target(类型声明)：若类型声明对应的是接口，则匹配所有实现了该接口的类，若是类，则匹配它本身及它的子类。例如：**@Pointcut("target(com.feint.kotlinaoptest.controller.MyController)")。**其中MyController是一个接口。
4.  this(类型声明)：和target用法差不多，只不过this对应的目标对象的一个代理，而target对应的是目标对象本身。
5.  args(类型声明)：匹配所有参数符合表达式的方法。这个例子中的Pointcut表达式也可以这么写："**args(javax.servlet.http.HttpServletRequest,..)&&@annotation(org.springframework.web.bind.annotation.RequestMapping)**"。意思是：匹配带有RequestMapping注解的第一个参数类型为HttpServletRequest的所有方法。
6.  @annotation(注解类声明)：匹配所有带有该注解的方法。例如：**@Pointcut("****@annotation(org.springframework.web.bind.annotation.RequestMapping)")。**
7.  @within(注解类声明)：匹配所有使用了该注解的类。例如：**@Pointcut("@within(org.springframework.web.bind.annotation.RestController)")**
8.  @agrs(注解类声明)：匹配参数带有指定注解的所有方法
9.  @target(注解类声明)：目标对象带有指定类型的注解，例如：**@Pointcut("within(com.feint.kotlinaoptest.controller..\*)&&@target(org.springframework.web.bind.annotation.RestController)")。**

附加说明：个人感觉within和this、target最大的区别就是，前者是根据表达式的模式来匹配相应的类；后者则是匹配指定类型本身或其子类和实现类。

通知也有三种类型：  


1.  前置通知：@Before 在被匹配的方法执行之前
2.  后置通知：@AfterReturning 在被匹配方法返回值之后
3.  异常通知：@AfterThrowing 在被匹配方法抛出异常后
4.  最终通知：@After 在被匹配的方法执行之后（不论是正常退出，还是因为异常退出）
5.  环绕通知：@Around 围绕被匹配方法的整个执行过程。其通知方法的第一个参数必须是ProceedingJoinPoint类型。

注意：这里解析‘ User－Agent ’信息，使用到一个第三方的库，需要添加以下依赖：  


> <dependency>
> 
> <groupId>eu.bitwalker</groupId>
> 
> <artifactId>UserAgentUtils</artifactId>
> 
> <version>1.20</version>
> 
> </dependency>

# 结果： #

访问在浏览器中访问服务接口后，可以在日志信息中看到，打印出了以下信息：  


> UserHost(ip=0:0:0:0:0:0:0:1, hostname=0:0:0:0:0:0:0:1, os=MAC\_OS\_X, browser=CHROME)

源码地址：https://github.com/feintKotlin/kotlin-aop-test


[Spring boot_Aop]: /pro/os/crawler/YREF-NJRF-RBF3.jpg
[Spring boot_Aop 1]: /pro/os/crawler/UZYB-BJI3-6JEZ.jpg
[Spring boot_Aop 2]: /pro/os/crawler/NM2Q-RZY2-MEJ2.jpg
 *  **原文作者：** K友
 *  **原文链接：** https://www.toutiao.com/item/6460963505167139341/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。