---
title: 移动端利用pdf.js实现在线预览pdf文档
date: 2018-06-26 15:38:01
categories: "开发"
tags:
	- 技术
	- HTML

---

项目中要求在移动端实现在线预览pdf文件，通过一番折腾，最后选择用pdf.js实现。

1、下载pdf.js

官网地址：https://mozilla.github.io/pdf.js/

2、各种配置

下载下来的文件包，就是一个demo，我们仿照这个demo做就可以啦

（1）页面元素如下：

``````````
<button class="product-term to-clause" id="noteDetail">《投保须知》</button><button class="to-clause" id="clauseDetail">《保险条款》</button>
``````````

（2）js代码如下：

$('\#clauseDetail').click(function () \{

window.open('../viewer.html?file=xxx-clause.pdf');

\});

注意：viewer.html就是下载下来文件包中的那个viewer.html，在此html中需要引入viewer.css、

locale.properties、pdf.js和viewer.js。修改viewer.js中的以下代码：

var DEFAULT\_URL = 'compressed.tracemonkey-pldi-09.pdf';修改为 var DEFAULT\_URL = '';

需要预览的pdf文件，就是**window**.open(**'../viewer.html?file=xxx-clause.pdf'**);中的xxx-clause.pdf文件。注意：pdf文件需要和

viewer.html放在同一个目录下，如果不在同一个目录下，需修改路径。

通过以上的配置，就实现了在线预览pdf文件。

![移动端利用pdf.js实现在线预览pdf文档][pdf.js_pdf]


[pdf.js_pdf]: /pro/os/crawler/MFRV-JMQQ-UFYE.jpg
 *  **原文作者：** 火到没朋友的大数据
 *  **原文链接：** https://www.toutiao.com/item/6571294299474887176/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。