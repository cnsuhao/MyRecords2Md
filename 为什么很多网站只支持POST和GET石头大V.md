---
title: 为什么很多网站只支持POST和GET
date: 2018-06-22 15:45:23
categories: "开发"
tags:
	- Tomcat
	- 技术
	- JSP
	- Windows 10

---

请求方法有很多，为什么很多网站只支持POST和GET呢？我们来了解一下。

**一、基于HTTP的请求方法**

根据HTTP标准，HTTP请求可以使用多种方法，其功能描述如下所示。

HTTP1.0定义了三种请求方法： GET、POST、HEAD

HTTP1.1新增了五种请求方法：OPTIONS、PUT、DELETE、TRACE 、CONNECT

![为什么很多网站只支持POST和GET][POST_GET]

请求方法

**二、不安全的请求方法**

GET、POST是最为常见方法，而且大部分主流网站只支持这两种方法，因为它们已能满足功能需求。GET方法主要用来获取服务器上的资源，而POST方法是用来向服务器特定URL的资源提交数据。而其它方法出于安全考虑被禁用，所以在实际应用中，九成以上的服务器都不会响应其它方法，并抛出404或405错误提示。以下列举几个HTTP方法的不安全性：

1、OPTIONS方法，将会造成服务器信息暴露，如中间件版本、支持的HTTP方法等。

![为什么很多网站只支持POST和GET][POST_GET 1]

OPTIONS

2、PUT方法，由于PUT方法自身不带验证机制，利用PUT方法即可快捷简单地入侵服务器，上传Webshell或其他恶意文件，从而获取敏感数据或服务器权限。

3、DELETE方法，利用DELETE方法可以删除服务器上特定的资源文件，造成恶意攻击。

**三、漏洞验证**

**a、环境搭建**

1、测试环境为：WIN10 64位、Tomcat 7.0.72、curl 7.49

2、在Tomcat 7默认配置中，web.xml文件的org.apache.catalina.servlets.DefaultServlet的

readonly参数默认是true，即不允许DELETE和PUT操作，所以通过PUT或DELETE方法访问，就会报403错误。为配合测试，把readonly参数设为false。

![为什么很多网站只支持POST和GET][POST_GET 2]

测试

**b、漏洞利用**

**1、PUT上传和DELETE删除文件成功**

在DefaultServlet的readonly参数为falsed的情况下，使用Curl进行测试，发现已能通过PUT上传和DELETE删除文件。

![为什么很多网站只支持POST和GET][POST_GET 3]

**2、直接PUT上传.jsp失败**

此时想直接上传webshell.jsp，但发现上传失败。

![为什么很多网站只支持POST和GET][POST_GET 4]

研究发现，原因是**在默认配置下，涉及jsp、jspx后缀名的请求由org.apache.jasper.servlet.JspServlet处理**，除此之外的请求才由org.apache.catalina.servlets.DefaultServlet处理。

![为什么很多网站只支持POST和GET][POST_GET 5]

刚才将DefaultServlet的readonly设置为false，并不能对jsp和jspx生效。因此，当PUT上传jsp和jspx文件时，Tomcat用JspServlet来处理请求，而JspServlet中没有PUT上传的逻辑，所以会403报错。

**3、利用漏洞成功上传WebShell**

对于不能直接上传WebShell的问题，一般的思路是通过解析漏洞来解决。

在此测试环境中，利用Tomcat 7的任意文件上传漏洞（CVE-2017-12615）来实现目的，该漏洞通过构造特殊后缀名，绕过tomcat检测，让它用DefaultServlet的逻辑处理请求，从而上传jsp文件。具体来说，主要有三种方法，比如shell.jsp%20 、shell.jsp::$DATA 、shell.jsp/

本次测试，使用第一种方法，在1.jsp后面加上%20，如此即可成功实现上传，并取得WebShell。

curl -X PUT http://127.0.0.1:8080/examples/1.jsp%20 -d “HelloJSP”

然后就直接挂马了，从下图可以看到成功上传webshell.jsp，并成功实现对服务器的控制。

![为什么很多网站只支持POST和GET][POST_GET 6]

测试

![为什么很多网站只支持POST和GET][POST_GET 7]

测试

**四、自查**

从上面的测试可以发现，虽然需在DefaultServlet的readonly参数为false前提下，才能实现渗透，但还是建议把除了GET、POST的HTTP方法禁止，有两方面原因：

 *  除GET、POST之外的其它HTTP方法，其刚性应用场景较少，且禁止它们的方法简单，即实施成本低。
 *  一旦让低权限用户可以访问这些方法，他们就能够以此向服务器实施有效攻击，即威胁影响大。

为什么要禁止除GET和POST外的HTTP方法，一是因为GET、POST已能满足功能需求，二是因为不禁止的话威胁影响大。可以使用OPTIONS方法遍历服务器使用的HTTP方法。但要注意的是，不同目录中激活的方法可能各不相同。而且许多时候，虽然反馈某些方法有效，但实际上它们并不能使用。即使OPTIONS请求返回的响应中没有列出某个方法，但该方法仍然可用。总的来说，建议手动测试每一个方法，确认其是否可用。比如：

**1、测试OPTIONS是否响应**，并是否有 Allow: GET, HEAD, POST, PUT, DELETE, OPTIONS

curl -v -X OPTIONS http://www.test.com/test/

**2、测试是否能通过PUT上传文件**

curl -X PUT http://www.test.com/test/test.html -d “test”

**3、找一个存在的文件，如test.txt，测试是否能删除**

curl -X DELETE http://www.example.com/test/test.text


[POST_GET]: /pro/os/crawler/2AUE-IZYV-IQYJ.jpg
[POST_GET 1]: /pro/os/crawler/QVU7-ZVQE-EZRI.jpg
[POST_GET 2]: /pro/os/crawler/2YJQ-BE7F-F7ZR.jpg
[POST_GET 3]: /pro/os/crawler/UN7V-RRBB-3Q2A.jpg
[POST_GET 4]: /pro/os/crawler/BBU3-6BV2-MIME.jpg
[POST_GET 5]: /pro/os/crawler/RQIQ-VVEN-BNMN.jpg
[POST_GET 6]: /pro/os/crawler/BMMM-YZUQ-AUU2.jpg
[POST_GET 7]: /pro/os/crawler/MJR2-6BNE-ZYJF.jpg
 *  **原文作者：** 石头大V
 *  **原文链接：** https://www.toutiao.com/item/6569811858201510413/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。