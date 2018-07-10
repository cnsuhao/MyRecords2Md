---
title: 轻量级rpc框架hessian的简单使用
date: 2017-03-26 00:20:52
categories: "开发"
tags:
	- 技术

---

我们在做jee项目的时候难免要调用别的系统，如果是不同项目之间常用的是webservise，而如果是内部项目则可以考虑使用hessian——因其相对简单、而且性能上要优于webservise。

下面我们通过一个简单的例子来说明。

**1、服务端：在服务端对hessian进行发布。**

我们要提供一个接口HelloService，并实现它HelloServiceImpl，如果接收的参数是对象，那么可以做一个类似Hello的JavaBean。

**（1）目录结构**

![轻量级rpc框架hessian的简单使用][rpc_hessian]

**（2）Hello.java**

![轻量级rpc框架hessian的简单使用][rpc_hessian 1]

**（3）HelloService.java**

![轻量级rpc框架hessian的简单使用][rpc_hessian 2]

**（4）HelloServiceImpl.java**

![轻量级rpc框架hessian的简单使用][rpc_hessian 3]

**（5）spring管理**

![轻量级rpc框架hessian的简单使用][rpc_hessian 4]

![轻量级rpc框架hessian的简单使用][rpc_hessian 5]

**二、客户端测试类**

调用是根据接口代理调用的，所以也要在客户端提供HelloService.java和组装参数的Hello.java，实际开发中可以是已jar包的方式提供。

![轻量级rpc框架hessian的简单使用][rpc_hessian 6]

执行既可以看到效果了！O(∩\_∩)O


[rpc_hessian]: /pro/os/crawler/7NA3-QYBQ-BMBI.jpg
[rpc_hessian 1]: /pro/os/crawler/EIAY-7BBE-IAJ3.jpg
[rpc_hessian 2]: /pro/os/crawler/FZMR-MVUA-IYRI.jpg
[rpc_hessian 3]: /pro/os/crawler/AUJA-NMIV-RFUR.jpg
[rpc_hessian 4]: /pro/os/crawler/MIFQ-JVU2-EYUV.jpg
[rpc_hessian 5]: /pro/os/crawler/Q3Y6-JF3I-MNIM.jpg
[rpc_hessian 6]: /pro/os/crawler/VMM2-IYJN-3UYE.jpg
 *  **原文作者：** java探讨
 *  **原文链接：** https://www.toutiao.com/item/6401251981943374338/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。