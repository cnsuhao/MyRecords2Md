---
title: 「前端 Zero系列」HTML文档（基本结构）
date: 2018-07-02 16:50:07
categories: "开发"
tags:
	- CSS
	- 技术
	- HTML5
	- HTML

---

完整的HTML文档主要包括三个部分：文档声明、文档头部、文档主体，它们构成了HTML的骨架结构。

![「前端 Zero系列」HTML文档（基本结构）][Zero_HTML]

青岛中新(83837777.com)

--------------------

# 文档声明 <!DOCTYPE> #

首行、顶格、大小写不敏感；

严格来说，它不是HTML标签的一部分，是一条声明指令，主要作用是告诉浏览器用哪种规范解析这份文档。

在HTML5之前有很多种类型，如HTML4.01 Strict.dtd(严格)、loose.dtd(松散)、frameset.dtd(框架)。严格型包含所有HTML元素和属性，但不包含展示性和弃用的元素(如font)，而过渡型或宽松型(loose)则包含展示性和弃用的元素。

HTML5，使用<!DOCTYPE html>，之前的标准不再使用。

![「前端 Zero系列」HTML文档（基本结构）][Zero_HTML 1]

青岛中新(83837777.com)

--------------------

# **文档头部，即head标签及其内容** #

若文档中忽略了head标签，则大部分浏览器会自动创建一个head元素。

头部描述了文档的一些基本属性和信息。头部内容，除了title和icon都不会作为真正的内容展现给用户。

根据HTML标准，仅有几个标签在头部是合法的：title、meta、base、link、style、script。

**\[1\] title**

文档标题，是head部分中唯一必需的元素。只可以包含文本，若是包含有标签，则包含的任何标签都不会被解释。

作用：定义浏览器工具栏中的标题、提供页面被添加到收藏夹时显示的标题、搜索引擎结果中的页面标题。

**\[2\] meta**

辅助标签，提供页面元信息(meta-information)，包括字符编码、搜索关键词、页面描述等。

meta标签里的内容并不显示，其目的是方便浏览器解析或利于搜索引擎搜索。

共有两个属性：http-equiv(用于模拟http响应头信息)和name(关于页面的描述信息)，通过“名/值”的方式实现。

name，一个文档级的元数据，将附着在整个页面上；http-equiv，一个编译指令，即由服务器指示页面应如何加载。

不同的属性的不同参数值，实现了不同的网页功能。

![「前端 Zero系列」HTML文档（基本结构）][Zero_HTML 2]

青岛中新(83837777.com)

**\[5\] style**

样式的引入也可以使用style文件直接添加样式。

style元素包含了文档的样式化信息或者文档的一部分，常用于引入内部CSS样式。

![「前端 Zero系列」HTML文档（基本结构）][Zero_HTML 3]

青岛中新(83837777.com)

--------------------

![「前端 Zero系列」HTML文档（基本结构）][Zero_HTML 4]

青岛中新(83837777.com)


[Zero_HTML]: /pro/os/crawler/AEUQ-RYRR-YYIR.jpg
[Zero_HTML 1]: /pro/os/crawler/UZEE-7FFM-RZZF.jpg
[Zero_HTML 2]: /pro/os/crawler/MUYU-QEIB-FEI3.jpg
[Zero_HTML 3]: /pro/os/crawler/ZUZV-ZRZB-RZNJ.jpg
[Zero_HTML 4]: /pro/os/crawler/MF6V-UJMB-A7N2.jpg
 *  **原文作者：** 青岛中新
 *  **原文链接：** https://www.toutiao.com/item/6573539390264443396/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。