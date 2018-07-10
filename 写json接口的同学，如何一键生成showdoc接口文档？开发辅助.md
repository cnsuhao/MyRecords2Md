---
title: 写json接口的同学，如何一键生成showdoc接口文档？
date: 2017-08-24 07:30:16
categories: "开发"
tags:
	- ECMAScript
	- Word
	- JavaScript
	- 编程语言
	- JSON

---

作为服务端开发人员，不管你使用什么语言进行接口开发，对外提供接口是必须的，写接口文档也是必须的。而在众多对外接口中JSON数据格式的接口所占比例很高。  


这里简单介绍一下什么是JSON？该介绍引自百度百科

> JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式。它基于 ECMAScript (w3c制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

写文档向来是开发人员最不乐意做的事情，由其是字段太多的接口文档。而不同的公司对接口文档的管理也不同，有用word写的，也有用excel写的，当然这两种接口文档作者均有写过。这些采用线下接口文档的形式不利于传播，不方便统一管理。现在很多开源的接口文档管理工具， 大多数基于web浏览器管理接口文档并支持markdown语法。以showdoc为例，该工具能够很好的呈现已按模板编写好的接口文档。编写时可以插入接口模板，但字段就得一个个手动添加了，要是对象字段过多，无疑是很繁琐的。先来看下showdoc编写接口的界面，此处已经插入了模板。

![写json接口的同学，如何一键生成showdoc接口文档？][json_showdoc]

插入模板之后需要一个个字段添加

而作为程序员就是要通过技术手段解决生活中繁琐而重复的劳动，下面介绍一下如何利用“开发辅助”工具快速生成showdoc下的JSON接口文档。  


所需工具：[开发辅助][Link 1] 该工具既调试了接口又生成了接口文档，一举两得。

操作步骤，以调用快递100接口为例：

 *  打开开发辅助，选择接口调试标签。
 *  选择接口请求方式，填写接口地址及参数。参数可跟url之后或填入请求参数栏。
 *  填写完成之后点击send按钮，等待接口返回之后查看showdoc标签页。
 *  在showdoc页签中右键，弹出菜单选择全选，再右键弹出菜单选择复制。
 *  将复制的内容直接粘贴到showdoc页面编辑器中，只需要再修改一下参数说明就好了，是不是省事很多。

操作步骤及结果如图所示：  


![写json接口的同学，如何一键生成showdoc接口文档？][json_showdoc 1]

请求返回之后查看showdoc页签

![写json接口的同学，如何一键生成showdoc接口文档？][json_showdoc 2]

全选及复制生成的接口文档内容

将内容粘贴到showdoc编辑器之后效果如下图所示：  


![写json接口的同学，如何一键生成showdoc接口文档？][json_showdoc 3]

请求参数已全部解析

![写json接口的同学，如何一键生成showdoc接口文档？][json_showdoc 4]

返回示例中列表只保留了一条数据

![写json接口的同学，如何一键生成showdoc接口文档？][json_showdoc 5]

返回参数已按层级全部解析

该工具下载地址：http://www.vbox.top


[json_showdoc]: /pro/os/crawler/RYAB-2M6B-VMU3.jpg
[Link 1]: http://www.toutiao.com/i6456286609380737550/
[json_showdoc 1]: /pro/os/crawler/QM7Z-7RVY-7V2M.jpg
[json_showdoc 2]: /pro/os/crawler/7ZNZ-VE63-ANRI.jpg
[json_showdoc 3]: /pro/os/crawler/MQFV-RFFI-VYA3.jpg
[json_showdoc 4]: /pro/os/crawler/INJZ-ZBYY-FRAF.jpg
[json_showdoc 5]: /pro/os/crawler/EE2I-QEQV-JVUR.jpg
 *  **原文作者：** 开发辅助
 *  **原文链接：** https://www.toutiao.com/item/6457611862992224782/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。