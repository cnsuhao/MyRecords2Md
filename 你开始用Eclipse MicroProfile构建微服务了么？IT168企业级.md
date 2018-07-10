---
title: 你开始用Eclipse MicroProfile构建微服务了么？
date: 2017-11-17 11:50:19
categories: "开发"
tags:
	- 技术
	- Eclipse
	- JSON
	- Gradle
	- 云计算

---

本文将快速向您展示如何使用最近更新的Eclipse MicroProfile API构建微服务。

Eclipse MicroProfile最近得到了很多关注，包括最新版本1.2，以及一些新增功能。本文是一个使用MicroProfile API构建微服务的快速教程。

**MicroProfile由JavaEE的核心技术构建而成：**

\* JAX-RS 2.0

\* CDI 1.2

\* JSON-P

**为了使微服务更适合云计算时代的需要，新版本不仅添加了一套规范，同时还包括：**

\* 配置管理

\* 容错

\* 指标

\* 健康检查

\* JWT授权

这些规范组成了Eclipse MicroProfile 1.2，这是API的第三个版本。那么应该如何使用呢?由于这只是一个规范，所以您需要检查供应商的特定需求。在大部分情况下，用户将继续像使用JavaEE一样部署WAR文件。首先，将MicroProfile依赖项添加到您的项目中。

**Maven：**

![你开始用Eclipse MicroProfile构建微服务了么？][Eclipse MicroProfile]

**Gradle：**

![你开始用Eclipse MicroProfile构建微服务了么？][Eclipse MicroProfile 1]

这个依赖项引入了构建应用程序所需的所有API，但是，用户可能需要参考服务器的运行以了解是否需要其他依赖项。那么典型的服务是什么样的呢?

1. 一个JAX-RS控制器。由于公开了REST API，所以需要一个控制器来处理API调用。

2. 某种服务。需要一些支持组件来生成或使用数据。

3. 监测。帮助掌握这个服务被调用的频率，以及每个请求需要多长时间。

4. 可配置性。希望以声明方式进行，而不是客户端指定数据量。

5. 安全。需要声明性和业务逻辑驱动的安全性，以了解如何响应请求。

6. 容错。关心消耗的任何服务，并确保可以快速舍弃或恢复。

首先是看起来非常熟悉的rest控制器：

![你开始用Eclipse MicroProfile构建微服务了么？][Eclipse MicroProfile 2]

进一步深入服务，可以看到可配置性的体现：

![你开始用Eclipse MicroProfile构建微服务了么？][Eclipse MicroProfile 3]

接下来，假设想处理书籍的创建和出版过程。支持的服务可能如下所示：

![你开始用Eclipse MicroProfile构建微服务了么？][Eclipse MicroProfile 4]

最后，如果管理作者是一个单独有界的上下文，那么可以将其表示为一个离散的服务，可以从客户端到该远程服务器来检查作者是否存在。

![你开始用Eclipse MicroProfile构建微服务了么？][Eclipse MicroProfile 5]

本教程使用了一些rest控制器、服务和一个用Eclipse MicroProfile构建的微服务来管理书籍。


[Eclipse MicroProfile]: /pro/os/crawler/V3QM-BJU3-MNYN.jpg
[Eclipse MicroProfile 1]: /pro/os/crawler/YZMB-QJ6J-FNIV.jpg
[Eclipse MicroProfile 2]: /pro/os/crawler/RMVI-VURN-ZFB3.jpg
[Eclipse MicroProfile 3]: /pro/os/crawler/E77J-VR3M-URVN.jpg
[Eclipse MicroProfile 4]: /pro/os/crawler/JFYJ-MFFE-IRMA.jpg
[Eclipse MicroProfile 5]: /pro/os/crawler/NVUN-MFJM-7JNM.jpg
 *  **原文作者：** IT168企业级
 *  **原文链接：** https://www.toutiao.com/item/6489225794655617549/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。