---
title: 一只注重审美，且性能高效的移动端和微信UI
date: 2018-01-24 07:07:15
categories: "开发"
tags:
	- CSS
	- 移动互联网
	- 编程语言
	- 微信
	- HTML

---

> 前端时间公司有个项目需要用到手机端和电脑端，我负责手机端开发，对于之前用到的UI都不是很合意，变在知乎上看到了有人介绍一款UI，变深入的了解了一下，发现和适合手机端开发应用。

这里介绍的就是**YDUI Touch，一只注重审美，且性能高效的移动端和微信UI**

![一只注重审美，且性能高效的移动端和微信UI][UI]

YDUI有两个版本，一种是基于vue.js的，另一版本是js+html+css，开发过程中可以根据自己需求选择版本，其用法有一定的差别，相对于vue版本的js+html版本的简单点。

![一只注重审美，且性能高效的移动端和微信UI][UI 1]

在使用YDUI之前，你应该熟悉HTML、CSS、JavaScript之外，还应该了解移动开发的相关特性，比如：兼容性、像素处理等。

# **YUDI特点** #

 *  可定制性高：可以通过自定义JavaScript组件，less文件、less变量定制一份属于自己的YDUI。

![一只注重审美，且性能高效的移动端和微信UI][UI 2]

![一只注重审美，且性能高效的移动端和微信UI][UI 3]

![一只注重审美，且性能高效的移动端和微信UI][UI 4]

 *  兼容绝大多数移动端设备（兼容Android4.0+、IOS6.0+）

# 使用 #

下载YDUI之后解压可以得到如下的文件目录结构，在head标签内引入path/build/css/ydui.css和path/build/js/ydui.flexible.js文件；

![一只注重审美，且性能高效的移动端和微信UI][UI 5]

> <!-- 引入YDUI样式 -->
> 
> <link rel="stylesheet" href="path/build/css/ydui.css" />
> 
> <!-- 引入YDUI自适应解决方案类库 -->
> 
> <script src="path/build/js/ydui.flexible.js"></script>

凡是涉及到YDUI组件的，引入jQuery2.0+和path/build/js/ydui.js，如下图一个Button组件

![一只注重审美，且性能高效的移动端和微信UI][UI 6]

![一只注重审美，且性能高效的移动端和微信UI][UI 7]

# 其他 #

 *  设备检测工具：YDUI.device提供了一些基本的设备侦测信息可供使用。如下

> console.log(YDUI.device)
> 
> //控制台打印出的信息
> 
> \{
> 
> isAndroid: false,
> 
> isIOS: false,
> 
> isIpad: false,
> 
> isIphone: false,
> 
> isIpod: false,
> 
> isMobile: false,
> 
> isWebView: false,
> 
> isWeixin: false,
> 
> isMozilla: false,
> 
> pixelRatio: 1
> 
> \}

 *  Rem 自适应类

ydui.flexible.js 是处理移动端 rem 自适应(可伸缩布局方案)的类库，无须第三方工具(如Sass/Less方法、Gulp、Sublime插件)，轻松口算设计稿对应rem值；

在所有页面加入ydui.flexible.js文件，把设计稿的尺寸换算成rem，rem计算公式

> 设计图尺寸px / 100 = 实际rem

例如设计稿宽度是100px那么在编写代码的时候就应该是1rem。

注意ydui.flexible.js文件中的docWidth为设计文档的宽度。

# 还有一个基于vue.js版本 #

![一只注重审美，且性能高效的移动端和微信UI][UI 8]

![一只注重审美，且性能高效的移动端和微信UI][UI 9]

在使用vue-ydui之前，必选先了解vue的相关基础知识和组件，引入有两种方法

 *  CDN引入

> <!-- 使用rem，需另外引入ydui.flexible.js自适应类库 -->
> 
> <link rel="stylesheet" href="//unpkg.com/vue-ydui/dist/ydui.rem.css">
> 
> <script src="//unpkg.com/vue-ydui/dist/ydui.flexible.js"></script>
> 
> <!-- 引入Vue2.x -->
> 
> <script src="//vuejs.org/js/vue.min.js"></script>
> 
> <!-- 引入组件库 -->
> 
> <script src="//unpkg.com/vue-ydui/dist/ydui.rem.js"></script>

 *  NPM安装

> $ npm install vue-ydui --save

在使用过程中可以根据需求，选择全局安装和按需安装。


[UI]: /pro/os/crawler/FBVV-VMBZ-BV6Z.gif
[UI 1]: /pro/os/crawler/YRNV-EZAA-ENQV.jpg
[UI 2]: /pro/os/crawler/FM7B-EJJQ-ZV2A.jpg
[UI 3]: /pro/os/crawler/EE3Y-BBIA-AAYE.jpg
[UI 4]: /pro/os/crawler/JUQY-R37V-E3AI.jpg
[UI 5]: /pro/os/crawler/JQZV-UYAR-MUQY.jpg
[UI 6]: /pro/os/crawler/QRFV-VUFN-QFFQ.jpg
[UI 7]: /pro/os/crawler/NR7F-JVFA-M7BQ.gif
[UI 8]: /pro/os/crawler/VIR7-NV2A-NBZM.jpg
[UI 9]: /pro/os/crawler/J7JI-FAYJ-YBRV.jpg
 *  **原文作者：** 创富说
 *  **原文链接：** https://www.toutiao.com/item/6514258543816737283/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。