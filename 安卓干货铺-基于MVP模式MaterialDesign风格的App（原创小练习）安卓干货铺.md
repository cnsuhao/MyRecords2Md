---
title: 安卓干货铺-基于MVP模式MaterialDesign风格的App（原创小练习）
date: 2017-10-14 10:14:12
categories: "开发"
tags:
	- 技术
	- 文章
	- 软件
	- GitHub

---

> **如果想了解APP的知识，直接私信我，可以交个朋友，另外置顶文章有IT资源合集，可以免费获取。**

前些日子写了一个小练习，由于时间比较紧，里面的如果有错误希望大家见谅。

这是一款Material Design风格的新闻App(视频图片),采用MVP模式,RxJava+Retrofit开发，当时项目中用的是rxjava1，有时间的话我会更新为rxjava2。

--------------------

![安卓干货铺-基于MVP模式MaterialDesign风格的App（原创小练习）][-_MVP_MaterialDesign_App]

![安卓干货铺-基于MVP模式MaterialDesign风格的App（原创小练习）][-_MVP_MaterialDesign_App 1]

![安卓干货铺-基于MVP模式MaterialDesign风格的App（原创小练习）][-_MVP_MaterialDesign_App 2]

--------------------

**部分知识介绍:**

1.项目运用了CoordinatorLayout+AppBarLayout+Toolbar实现上下滚动处理特殊效果。

2.DrawerLayout+NavigationView实现侧滑+设置菜单

3.MVP+rxjava+Retrofit封装 ，我github上有封装。

4.下拉刷新，上拉加载

5.FloatingActionButton悬浮按钮。

6.CardView实现卡片式条目。

--------------------

**项目中的开源库**

RxJava

Rxandroid

Retrofit

ButterKnife

PhotoView 图片放大缩放

PullLoadMoreRecyclerView -下拉刷新上拉加载

NiceVieoPlayer -视频

MVP\_Retrofit\_RxJava封装

> **如果想了解APP的知识，直接私信我，可以交个朋友，另外置顶文章有IT资源合集，可以免费获取。**  
> 


[-_MVP_MaterialDesign_App]: /pro/os/crawler/3MQM-IIEB-JFUY.gif
[-_MVP_MaterialDesign_App 1]: /pro/os/crawler/UYFM-UB73-QBR2.jpg
[-_MVP_MaterialDesign_App 2]: /pro/os/crawler/V7ZY-AUEZ-B3IA.jpg
 *  **原文作者：** 安卓干货铺
 *  **原文链接：** https://www.toutiao.com/item/6476583723922883086/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。