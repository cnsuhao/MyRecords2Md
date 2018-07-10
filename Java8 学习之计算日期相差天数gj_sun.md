---
title: Java8 学习之计算日期相差天数
date: 2016-11-26 17:33:13
categories: "开发"
tags:
	- 学习笔记

---

昨天在公司遇到一个小问题，需要计算两个日期之间相差的天数，于是首先想到的就是老API中的写法：

## ![代码][YVMV-RERA-67BY.jpg] ##

后来觉得都有java8 还这样用是不是有点out了，于是就去翻time包中的方法，看到一个方法，这个方法是  
Period中的between方法，又看到了getDays方法，感觉找到地方了，那就试试呗。  
![代码][RVQ2-EA32-AMMF.jpg]

结果也挺好：  
![结果][Q3M6-N26Z-BMBE.jpg]  
但是我换了个日期就发现问题了：  
![代码+结果][MFM6-BEUI-NVUU.jpg]

结果并非我想要的，好吧，这种写法不行。那我只好继续找，这次终于找到想要的方法了  
![doc][]

虽然英语不太好，但是这个还是看懂了，把日期转换成天了，于是就试试了：  
![代码][VEQB-QRRN-UAFE.jpg]

最终方法改成了这样：  
![代码][JAMF-BEJI-NENV.jpg]

学习之路还很漫长，以后有机会要多写写！本人新手，有错的地方望大神指正！


[YVMV-RERA-67BY.jpg]: /pro/os/crawler/YVMV-RERA-67BY.jpg
[RVQ2-EA32-AMMF.jpg]: /pro/os/crawler/RVQ2-EA32-AMMF.jpg
[Q3M6-N26Z-BMBE.jpg]: /pro/os/crawler/Q3M6-N26Z-BMBE.jpg
[MFM6-BEUI-NVUU.jpg]: /pro/os/crawler/MFM6-BEUI-NVUU.jpg
[doc]: /pro/os/crawler/FZMI-ENVF-QBJZ.jpg
[VEQB-QRRN-UAFE.jpg]: /pro/os/crawler/VEQB-QRRN-UAFE.jpg
[JAMF-BEJI-NENV.jpg]: /pro/os/crawler/JAMF-BEJI-NENV.jpg
 *  **原文作者：** gj_sun
 *  **原文链接：** https://blog.csdn.net/gj_sun/article/details/53353446
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。