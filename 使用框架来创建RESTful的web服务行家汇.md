---
title: 使用框架来创建RESTful的web服务
date: 2017-07-11 21:19:55
categories: "开发"
tags:
	- Pages
	- Excel
	- XML
	- 编程语言
	- JSON

---

想要使用Express、restify或者其他框架来创建RESTful的web服务。使用正确的http方法、URLs和头部信息来创建语义化的RESTful API。REST即为表征性状态传输，记住这个并不是特别有用，除非你想在面试中打动别人。web开发人员讨论它时通常会和SOAP（简单对象访问协议）做对比，通常被视为是更有协作性和更严谨的一种创建web APIs的方式。事实上，REST API是相对严格的，但是关键的区别在于REST在基础的层面上拥抱http——HTTP方法本身就具备了语义。如果你曾经做过基础表单，应该对GET和POST请求很熟悉。在REST中，这些http动词有着特定的含义。例如，POST用于创建资源，GET表示获取资源。Node开发者通常使用JSON来创建API。在Node中，JSON是最便于生成和读取的结构化数据格式，它同时在客户端的JavaScript也能很好地工作。但是REST不意味着JSON——你可以自由地使用任意数据格式。一些客户端和服务使用XML，我们甚至看到过使用CSV和电子表格格式，如Excel。

所期待的数据格式可以在请求的Accept头部中指定。对于JSON，它应该是application/json，而XML，则是application/xml。还有其他有用的请求头部——Accept-Version可以用于请求不同版本的API。这允许客户端来使用自己支持的版本，同时你可以随意地升级服务器而不破坏向后兼容性——你总是可以在人们更新客户端之前更快地更新服务器程序。

Express基于http核心模块提供了一个轻量的封装层，但它不包括任何数据持久化，除了在内存中的sessions和cookies。你必须决定使用哪个数据库和数据库模块。restify也是如此：它不会自动从http中映射数据来做离线存储，你需要自己找个方式来处理。

Restify和Express十分相似。其中不同的是Express有构建web应用的特性，包括了渲染模块。相反地，restify致力于构建REST APIs，这带来了不同的需求。Restify可以让使用语义化的http版本头部信息来处理多个版本的APIs变得容易，并且拥有基于事件的API来触发和监听和http相关的事件和错误。它也支持节流，你可以控制多快来处理一次响应。

下图展示了一个典型的RESTful API，允许\*page\*对象创建、读取、更新和删除。

![使用框架来创建RESTful的web服务][RESTful_web]

要开始构建REST APIs，应该考虑你的对象是怎么样的。想象一下你在构建一个内容管理系统：可能有页面、用户和图片。如果要添加一个按钮来允许页面在“发布”和“草稿”之间切换，可能需要一个REST API，它支持这样的请求：PATCH/pages/:id，你可以把按钮和一些客户端的JavaScript绑定，或者是一个表单携带\{state:'published'\}或\{state:'draft'\}数据post提交到/pages/:id。如果你面临的是一个Express应用程序，只有PUT/pages/:id，那么可能需要把现有的实现改为使用PATCH的代码。


[RESTful_web]: /pro/os/crawler/YB7B-IBVR-7BIQ.jpg
 *  **原文作者：** 行家汇
 *  **原文链接：** https://www.toutiao.com/item/6441502594723480077/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。