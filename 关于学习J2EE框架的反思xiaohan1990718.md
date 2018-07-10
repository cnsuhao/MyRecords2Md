---
title: 关于学习J2EE框架的反思
date: 2011-12-25 11:44:39
categories: "开发"
tags:
	- java

---

现在许许多多的初学者和程序员，都在趋之若鹜地学习Web开发的宝典级框架：Struts2，Spring，Hibernate。似乎这些框架成为了一个人是否精通Java，是否会写J2EE程序的唯一事实标准和找工作的必备基础。

然而，如果在面试的时候问这些程序员，你们为什么要学习这些框架？这些框架的本质到底是什么？似乎很少很少有人能够给我非常满意的答复。因为他们都 在为了学习而学习，为了工作而学习，而不是在真正去深入了解一个框架。其实所有的人都应该思考这样的问题：为什么要学习框架？框架到底给我带来了什么？接 下来，我们以登录作为一个最简单的例子，来看看不同的年代，我们是怎么写Web程序的。

后来，我们放弃了在页面上写逻辑。

后来，程序写得越来越多，我们发现，这种在HTML代码中编写Java代码来完成逻辑的方式存在着不少问题：

1. Java代码由于混杂在一个HTML环境中而显得混乱不堪，可读性非常差。一个JSP文件有时候会变成几十K，甚至上百K。要找一段逻辑，经常无法定位。

2. 编写代码时非常困惑，不知道代码到底应该写在哪里，也不知道别人是不是已经曾经实现过类似的功能，到哪里去引用。

3. 突然之间，某个需求发生了变化。于是，每个人蒙头开始全程替换，还要小心翼翼的，生怕把别人的逻辑改了。

4. 逻辑处理程序需要自己来维护生命周期，对于类似数据库事务、日志等众多模块无法统一支持。

在这个时候，如果有一个产品，它能够将页面上的那些Java代码抽取出来，让页面上尽量少出现Java代码，该有多好。于是许多人开始使用servlet来处理那些业务逻辑。

``````````

``````````

1.  publicclass LoginServlet extends HttpServlet \{ 
2.  /\* (non-Javadoc) 
3.   \* @see javax.servlet.http.HttpServlet\#doPost(javax.servlet.http.HttpServletRequest, javax.servlet.http.HttpServletResponse) 
4.   \*/
5.  @Override
6.  protectedvoid doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException \{ 
7.   String message = null; 
8.   RequestDispatcher dispatcher = req.getRequestDispatcher("/result.jsp"); 
9.   String name = req.getParameter("name"); 
10.  String password = req.getParameter("password"); 
11. 
12.  UserHandler userHandler = new UserHandler(); 
13. if(userHandler.authenticate(name, password)) \{ 
14.  message = "恭喜你，登录成功"; 
15.  \} else \{ 
16.  message = "对不起，登录失败"; 
17.  \} 
18.  req.setAttribute("message", message); 
19.  dispatcher.forward(req, resp); 
20.  \} 
21. \} 

在这里，我们需要在web.xml中为这个servlet配置url的请求关系。

``````````

``````````

1.  <servlet>
2.  <servlet-name>Login</servlet-name>
3.  <servlet-class>
4.   com.demo2do.servlet.LoginServlet 
5.  </servlet-class>
6.  </servlet>
7.  <servlet-mapping>
8.  <servlet-name>Login</servlet-name>
9.  <url-pattern>
10.  /Login 
11. </url-pattern>
12. </servlet-mapping>

代码重构到这里，我们发现，其实我们的工作量本身并没有减少，只是代码从JSP移动到了Servlet，使得整个流程看上去稍微清楚了一些。然而，为了这么点干净，我们付出的代价是什么？为每个servlet都在web.xml里面去做一个url的请求配置！　

在很多年前，我们这么写程序的。

很多年前，那是一个贫苦的年代，如果我们要使用Java在网页上做一些动态的交互功能。很多人会告诉你一个技术，叫做JSP。在我还对Java非常困惑的时候，就有人告诉我，JSP是个好东西，它可以在HTML代码里面写Java代码来完成逻辑。

``````````

``````````

1.  <% 
2.   String name = request.getParameter("name"); 
3.   String password = request.getParameter("password"); 
4.  
5.   UserHandler userHandler = new UserHandler(); 
6.  if(userHandler.authenticate(name, password)) \{ 
7.  %> 
8.  <p>恭喜你，登录成功</p> 
9.  <% 
10.  \} else \{ 
11. %> 
12. <p>对不起，登录失败</p> 
13. <% 
14.  \} 
15. %> 

作为一张JSP，它可以接收从别的JSP发送过来的登录请求，并进行处理。这样，我们不需要任何额外的配置文件，也不需要任何框架的帮忙，就能完成逻辑。

再后来，出现框架。

时代进一步发展，人们发现简单的JSP和Servlet已经很难满足人们懒惰的要求了。于是，人们开始试图总结一些公用的Java类，来解决Web 开发过程中碰到的问题。这时，横空出世了一个框架，叫做struts。它非常先进地实现了MVC模式，成为了广大程序员的福音。

struts的代码示例我就不贴了，网上随便搜搜你可以发现一堆一堆的。在一定程度上，struts能够解决web开发中的职责分配问题，使得显示 与逻辑分开。不过在很长一段时间内，使用struts的程序员往往无法分别我们到底需要web框架帮我们做什么，我们到底需要它完成点什么功能？

我们到底要什么？

在回顾了我们写代码的历史之后，我们回过头来看看，我们到底要什么？

无论是使用JSP，还是使用Struts1，或是Struts2，我们至少都需要一些必须的元素（如果没有这些元素，或许我还真不知道这个程序会写成什么样子）：

**1. 数据**

在这个例子中，就是name和password。他们共同构成了程序的核心载体。事实上，我们往往会有一个User类来封装name和password，这样会使得我们的程序更加OO。无论怎么说，数据会穿插在这个程序的各处，成为程序运行的核心。

**2.页面展示**

在这个例子中，就是login.jsp。没有这个页面，一切的请求、验证和错误展示也无从谈起。在页面上，我们需要利用HTML，把我们需要展现的数据都呈现出来。同时我们也需要完成一定的页面逻辑，例如，错误展示，分支判断等等。

**3.处理具体业务的场所**

在这里，不同阶段，处理具体业务的场所就不太一样。原来用JSP和Servlet，后来用Struts1或者Struts2的Action。

上面的这些必须出现的元素，在不同的年代，被赋予了不同的表现形式，有的受到时代的束缚，其表现形式非常落后，有的已经不再使用。但是拨开这些外在的表现形式，我们就可以发现，这不就是我们已经熟门熟路的MVC嘛？

数据 —— Model

页面展示 —— View

处理具体业务的场所 —— Control

**所以，框架不重要，概念是王道。只要能够深刻理解MVC的概念，框架对你来说，只是一个jar包而已。**

MVC的概念其实就那么简单，这些概念其实早已深入我们的内心，而我们所缺乏的是将其本质挖掘出来。我们来看看下面这幅图，这是一副流行了很多年的讲述MVC模型的图：

[![A2UE-2YYF-M3AA.jpg][]][A2UE-2YYF-M3AA.jpg]

在这幅图中，MVC三个框框各司其职，结构清晰明朗。不过我觉得这幅图忽略了一个问题，就是数据是动的，数据在View和Control层一旦动起来，就会产生许多的问题：

1. 数据从View层传递到Control层，如何使得一个个扁平的字符串，转化成一个个生龙活虎的Java对象。

2. 数据从View层传递到Control层，如何方便的进行数据格式和内容的校验？

3. 数据从Control层传递到View层，一个个生龙活虎的Java对象，又如何在页面上以各种各样的形式展现出来。

4. 如果你试图将数据请求从View层发送到Control层，你如何才能知道你要调用的究竟是哪个类，哪个方法？一个Http的请求，又如何与Control层的Java代码建立起关系来？

除此之外，Control层似乎也没有想象中的那么简单，因为它作为一个控制器，至少还需要处理以下的问题：

1. 作为调用逻辑处理程序的facade门面，如果逻辑处理程序发生了异常，我们该如何处理？

2. 对于逻辑处理的结果，我们需要做怎么样的处理才能满足丰富的前台展示需要？

这一个又一个问题的提出，都基于对MVC的基本概念的挖掘。所以，这些问题都需要我们在写程序的时候去一一解决。说到这里，这篇文章开头所提的问题应该可以有答案了：**框架是为了解决一个又一个在Web开发中所遇到的问题而诞生的。不同的框架，都是为了解决不同的问题，但是对于程序员而言，他们只是jar包而已。框架的优 缺点的评论，也完全取决于其对问题解决程度和解决方式的优雅性的评论。所以，千万不要为了学习框架而学习框架，而是要为了解决问题而学习框架，这才是一个 程序员的正确学习之道。**

原文链接：http://www.blogjava.net/lujie2012/archive/2011/11/25/j2ee.html


[A2UE-2YYF-M3AA.jpg]: /pro/os/crawler/A2UE-2YYF-M3AA.jpg