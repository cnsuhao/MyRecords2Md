---
title: 用vue+elementUI写分页效果
date: 2018-06-05 15:25:03
categories: "开发"
tags:
	- 技术

---

elementUI上面自带表格的分页效果,但是我写的用不到表格,就有点尴尬了,百度找了好多方法,有点复杂,

后来根据我自己的情况写了一个,比较简单一点的.

因为我里面的内容是循环遍历出来的 用v-if 加个条件 就能达到分页的效果了.下面附代码

![用vue+elementUI写分页效果][vue_elementUI]

pagesize=最大显示条数 currentPage档前页

假如 pagesize=4 currentPage=2

index=4 index<8 index=4.5.6.7 索引是从0开始 实际输入 5.6.7.8四条数据

![用vue+elementUI写分页效果][vue_elementUI 1]

分页器用的 elementUI上面的

![用vue+elementUI写分页效果][vue_elementUI 2]

1

page-sizes 是控制显示条数

![用vue+elementUI写分页效果][vue_elementUI 3]

设置默认当前页和当前显示条数

![用vue+elementUI写分页效果][vue_elementUI 4]

控制当前显示条数和当前页

新手上路,还请老司机多多照拂


[vue_elementUI]: /pro/os/crawler/7J3A-UVMN-JJFF.jpg
[vue_elementUI 1]: /pro/os/crawler/IJAR-V3YM-V6NU.jpg
[vue_elementUI 2]: /pro/os/crawler/J6NA-63QJ-AARM.jpg
[vue_elementUI 3]: /pro/os/crawler/ZNF7-NVE2-IAMV.jpg
[vue_elementUI 4]: /pro/os/crawler/EZNR-ZM3U-MUAA.jpg
 *  **原文作者：** 时间会留下最真的人
 *  **原文链接：** https://www.toutiao.com/item/6563498040701747716/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。