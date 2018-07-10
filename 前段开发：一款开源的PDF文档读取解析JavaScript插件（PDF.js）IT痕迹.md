---
title: 前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）
date: 2017-10-24 15:41:52
categories: "开发"
tags:
	- Mozilla
	- JavaScript
	- 编程语言
	- HTML
	- Apache

---

> PDF.js是一个开源（Apache-2.0协议）、通用、web标准平台解析和呈现pdf文档的插件，这是在HTML5下诞生的，它可以实现在HTML页面上直接显示PDF文档，支持主流浏览器，但在浏览器上功能展现可能有所不同。

![前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）][PDF_JavaScript_PDF.js]

1、快速开始

**https://github.com/mozilla/pdf.js**

下载pdf.js文件，下载完成后解压放到项目任意位置，在页面中引入pdf.js；也可以使用cdn引入。

![前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）][PDF_JavaScript_PDF.js 1]

在body中创建一个canvas标签的pdf.js容器。

![前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）][PDF_JavaScript_PDF.js 2]

运行以下脚本，一个简单pdf文档解析就完成了，可以根据自己的需求更改样式、配置等。  


![前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）][PDF_JavaScript_PDF.js 3]

![前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）][PDF_JavaScript_PDF.js 4]

更多例子请前往官网查看详情，可以让你快速简单的了解pdf.js。

**http://mozilla.github.io/pdf.js/examples/**

2、快捷键

![前段开发：一款开源的PDF文档读取解析JavaScript插件（PDF.js）][PDF_JavaScript_PDF.js 5]

pdf.js内置快捷键，支持使用快捷键简单的来查看pdf文档。

 *  导航：  
    

下一页：n、j、→

上一页：p、k、←

home首页、end尾页

 *  视图控制：

除了ctrl+滚轮 放大缩小外，还有以下快捷键

放大：ctrl + +, ctrl + =

缩小：ctrl + -

重置大小：ctrl + 0

顺时针旋转：r

逆时针旋转：shift + r

全屏：ctrl + alt + p

快捷键可以进行相应的配置，以上快捷键不是全部，只是一些基础的。

3、其他  


pdf.js目前没有API文档，我也就没有了解更多的内容，需要深入理解学习需要看源码，他当中有不少配置参数、事件等，需要的功能可以去找找看，没有还可以自己扩展。

在线演示：  


**http://mozilla.github.io/pdf.js/web/viewer.html**

官方网站：

**http://mozilla.github.io/pdf.js/**

--------------------

有哪里写得不好的地方请大家提出来，请轻喷，谢谢。 同时有什么与编程相关的好东西可以推举给我，再次感谢！

感谢@曹巍的小爬虫 提供的信息。


[PDF_JavaScript_PDF.js]: /pro/os/crawler/IQRJ-IYAJ-U2EA.jpg
[PDF_JavaScript_PDF.js 1]: /pro/os/crawler/UM7J-MFZA-7NNA.jpg
[PDF_JavaScript_PDF.js 2]: /pro/os/crawler/2YZI-BJEE-J22U.jpg
[PDF_JavaScript_PDF.js 3]: /pro/os/crawler/2MYV-BBNM-EY2Q.jpg
[PDF_JavaScript_PDF.js 4]: /pro/os/crawler/QJI7-NJEB-FJUB.jpg
[PDF_JavaScript_PDF.js 5]: /pro/os/crawler/MN7B-A3A3-UVYF.jpg
 *  **原文作者：** IT痕迹
 *  **原文链接：** https://www.toutiao.com/item/6473440405521170957/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。