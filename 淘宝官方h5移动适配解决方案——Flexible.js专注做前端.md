---
title: 淘宝官方h5移动适配解决方案——Flexible.js
date: 2018-04-03 23:05:30
categories: "开发"
tags:
	- 技术
	- 登山车
	- HTML
	- 电子商务

---

我承认是刚才了解到淘宝官方h5移动适配解决方案——Flexible.js 的，在此之前，一直是用 rem+px混用，加上css3多媒体查询来做自适应的，效果也很好。当然，我们切图网也有我们自己的移动适配解决方案，这个暂且不提，现在我们聊聊 Flexible.js 。

flexible.js 的用法非常的简单，在页面的<head></head>中引入 flexible\_css.js,flexible.js文件：

// 加载阿里CDN的文件

<script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible\_css.js,flexible.js"></script>

下面我将以750px的设计稿为例，分析如何制作一个适配多终端的页面

通过ps获得顶部的高度，在750设计稿上96px,所以我们要用96/(750/10)，得到对应的rem值。

![淘宝官方h5移动适配解决方案——Flexible.js][h5_Flexible.js]

这样就实现了最终想要的结果，是不是很简单。

事实上 flexible.js 做了下面三件事：

 *  动态改写标签
 *  给<html>元素添加data-dpr属性，并且动态改写data-dpr的值
 *  给<html>元素添加font-size属性，并且动态改写font-size的值

\--

切图网（qietu.com）始于2007年，专注web前端开发、培训

wx：dingxiangming82


[h5_Flexible.js]: /pro/os/crawler/ZVBU-FY73-MVFI.jpg
 *  **原文作者：** 专注做前端
 *  **原文链接：** https://www.toutiao.com/item/6540238460798632455/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。