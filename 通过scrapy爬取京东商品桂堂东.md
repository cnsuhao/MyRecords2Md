---
title: 通过scrapy爬取京东商品
date: 2017-11-07 16:57:39
categories: "开发"
tags:
	- 网络爬虫
	- Scrapy
	- 编程语言
	- 京东
	- Python

---

# 什么是scrapy #

Scrapy，Python开发的一个快速、高层次的屏幕抓取和web抓取框架，用于抓取web站点并从页面中提取结构化的数据。（https://scrapy.org/）

# Scrapy安装 #

您可以使用pip来安装Scrapy(推荐使用pip来安装Python package).

    pip install Scrapy

# 创建项目    #

在开始爬取之前，您必须创建一个新的Scrapy项目。 进入您打算存储代码的目录中，运行下列命令:

    scrapy startproject jd

该命令将会创建包含下列内容的 jd 目录:

    jd/scrapy.cfgjd/__init__.pyitems.pypipelines.pysettings.pyspiders/__init__.py...

这些文件分别是:

scrapy.cfg: 项目的配置文件

jd/: 该项目的python模块。之后您将在此加入代码。

jd/items.py: 项目中的item文件.

jd/pipelines.py: 项目中的pipelines文件.

jd/settings.py: 项目的设置文件.

jd/spiders/: 放置spider代码的目录

# scrapy爬取京东商品代码实现 #

1. 代码结构

![通过scrapy爬取京东商品][scrapy]

代码结构

代码结构

2.JdSpider.py

Spider是用户编写用于从单个网站(或者一些网站)爬取数据的类。

其包含了一个用于下载的初始URL，如何跟进网页中的链接以及如何分析页面中的内容， 提取生成 item 的方法。

为了创建一个Spider，您必须继承 scrapy.Spider 类， 且定义以下三个属性:

name: 用于区别Spider。 该名字必须是唯一的，您不可以为不同的Spider设定相同的名字。

start\_urls: 包含了Spider在启动时进行爬取的url列表。 因此，第一个被获取到的页面将是其中之一。 后续的URL则从初始的URL获取到的数据中提取。

parse() 是spider的一个方法。 被调用时，每个初始URL完成下载后生成的 Response 对象将会作为唯一的参数传递给该函数。 该方法负责解析返回的数据(response data)，提取数据(生成item)以及生成需要进一步处理的URL的 Request 对象。

![通过scrapy爬取京东商品][scrapy 1]

JdSpider.py

3.item.py

Item 是保存爬取到的数据的容器；其使用方法和python字典类似， 并且提供了额外保护机制来避免拼写错误导致的未定义字段错误。

类似在ORM中做的一样，您可以通过创建一个 scrapy.Item 类， 并且定义类型为 scrapy.Field 的类属性来定义一个Item。 (如果不了解ORM, 不用担心，您会发现这个步骤非常简单)

![通过scrapy爬取京东商品][scrapy 2]

item.py

4.pipeline.py

当Item在Spider中被收集之后，它将会被传递到Item Pipeline，一些组件会按照一定的顺序执行对Item的处理。

每个item pipeline组件(有时称之为“Item Pipeline”)是实现了简单方法的Python类。他们接收到Item并通过它执行一些行为，同时也决定此Item是否继续通过pipeline，或是被丢弃而不再进行处理。

以下是item pipeline的一些典型应用：

清理HTML数据

验证爬取的数据(检查item包含某些字段)

查重(并丢弃)

将爬取结果保存到数据库中

目前的例子中我们将数据保存在xlsx文件中：  


![通过scrapy爬取京东商品][scrapy 3]

pipeline.py

5.setting.py

![通过scrapy爬取京东商品][scrapy 4]

setting.py

6.run.py

![通过scrapy爬取京东商品][scrapy 5]

run.py

# 运行爬虫    #

> python run.

# 执行结果    #

生成jd.xlsx文件  


![通过scrapy爬取京东商品][scrapy 6]

jd.xlsx


[scrapy]: /pro/os/crawler/BVRZ-BIFR-IMJM.jpg
[scrapy 1]: /pro/os/crawler/FQEF-IIBF-REMJ.jpg
[scrapy 2]: /pro/os/crawler/R2UY-NVIR-FFJB.jpg
[scrapy 3]: /pro/os/crawler/BBBE-NFYE-ZZJ3.jpg
[scrapy 4]: /pro/os/crawler/VR32-IIIJ-JEA2.jpg
[scrapy 5]: /pro/os/crawler/3IFR-ZFRF-QFAN.jpg
[scrapy 6]: /pro/os/crawler/BM3Y-IVU6-B7N2.jpg
 *  **原文作者：** 桂堂东
 *  **原文链接：** https://www.toutiao.com/item/6485579380834697741/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。