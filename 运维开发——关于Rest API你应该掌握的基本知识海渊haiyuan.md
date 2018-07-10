---
title: 运维开发——关于Rest API你应该掌握的基本知识
date: 2018-04-03 23:04:44
categories: "开发"
tags:
	- 技术
	- XML
	- 软件
	- JSON

---

# 基本概念说明  #

目前我们在各种官网平台上看到的API文档实际上应该是被叫做REST API。

分别进行说明：

API（Application Programming Interface,应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。

API 接口属于一种操作系统或程序接口，GUI接口属于一种图形操作系统。两者都属于直接用户接口。有时公司会将 API 作为其公共开放系统。也就是说，公司制定自己的系统接口标准，当需要执行系统整合、自定义和程序应用等操作时，公司所有成员都可以通过该接口标准调用源代码，该接口标准被称之为开放式API。

REST是Representational State Transfer的缩写，是用来描述创建HTTP API的标准方法的，这四种常用的行为（查看（view），创建（create），编辑（edit）和删除（delete））都可以直接映射到HTTP 中已实现的GET,POST,PUT和DELETE方法。

查阅各个大公司的开放平台，我们不难发现，都是Rest API，都是HTTP请求，响应报文都是大同小异的XML或者是JSON等众多雷同的特点。

是用固定的URI和可变的参数访问某个服务，来完成一系列业务请求。

• 每一个URI代表一种资源；

• 客户端和服务器之间，传递这种资源的某种表现层；

• 客户端通过几个HTTP动词，对服务器端资源进行操作，实现”表现层状态转化”。

# Rest API 格式 #

Rest API，无论它的名字多么高大上，它本质还是一个HTTP请求，POST也好，GET也罢，都是不同的数据提交方式。所以，能够决定一个Rest API的也就：URI、参数、请求方式、请求头等。

我们一般用URI来定义希望对外暴露的服务。结构基本类似 schema://yourCompanyDomain/rest/\{version\}/\{application\}/\{someService\}。schema可以是http，也可以是https，version指的是你这个API的版本，application一般会指向底层的某个子系统，someService就是这个子系统对外提供的服务。当然，如果按照业务为边界划分，也可将业务维度相同但隶属于底层不同的系统的服务定义为一个application。

对于这种Rest请求，常见的响应结果就是XML或者是JSON形式，往往结果中会包含请求状态，和时间戳，业务系统响应结果。

以上就是我认为对API需要了解的基本知识，如有不当之处，欢迎指出。如果想进一步深入了解Rest API相关知识的朋友，可以再进行深入的了解。
 *  **原文作者：** 海渊haiyuan
 *  **原文链接：** https://www.toutiao.com/item/6540238263657955843/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。