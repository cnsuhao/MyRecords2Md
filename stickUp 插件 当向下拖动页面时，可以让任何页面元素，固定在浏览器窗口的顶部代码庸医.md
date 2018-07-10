---
title: stickUp 插件 当向下拖动页面时，可以让任何页面元素，固定在浏览器窗口的顶部
date: 2017-07-11 16:50:55
categories: "开发"
tags:
	- 科技
	- WordPress
	- jQuery

---

stickUp 插件 当向下拖动页面时，可以让任何页面元素，固定在浏览器窗口的顶部。  
GitHub地址：https://github.com/LiranCohen/stickUp

简单使用方法：

1.  **引用JS（jquery 和stickUp）**
    
    <script src="js/jquery.min.js"></script>  
    
    
    <script src="js/stickUp.min.js"></script>
2.  **编写JAVASCRIPT实现**

jQuery(function($) \{

$(document).ready( function() \{

$('.navbar-wrapper').stickUp();

\});

\});

**效果图**

![stickUp 插件 当向下拖动页面时，可以让任何页面元素，固定在浏览器窗口的顶部][stickUp _]  


**效果图**

当然stickUp还不止这各功能，还可以实现单页导航

所谓的单页面导航就是针对单个页面的导航（这个解释好像有点弱智），就是页面滚动到那个区域的时候，导航栏菜单高亮

使用方法如下：

1.  给导航的菜单连接加上锚链接如：\#home  
    
2.  给页面元素赋予 **"id=home"**的属性
3.  JS代码

jQuery(function($) \{

$(document).ready( function() \{

$('.navbar-wrapper').stickUp(\{

parts: \{

0:'home',

1:'features',

2: 'news',

3: 'installation',

4: 'one-pager',

5: 'extras',

6: 'wordpress',

7: 'contact'

\},

itemClass: 'menuItem', itemHover: 'active' \});

\});

\});


[stickUp _]: /pro/os/crawler/Y7FA-QEQZ-I6RI.jpg
 *  **原文作者：** 代码庸医
 *  **原文链接：** https://www.toutiao.com/item/6441433011371115021/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。