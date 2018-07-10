---
title: Highcharts最全的API参考
date: 2014-02-21 09:07:15
categories: "开发"
tags:
	- chart

---

刚开始接触Highcharts的时候,去官网,可是死活打不开.不知道THE GREAT WALL对它做了什么...

昨日碰巧进去了~附上官网提供的在线API ：http://api.highcharts.com/highcharts\#chart.events

该API分两部分,一部分是配置参数说明,第二部分是对外提供的公共 方法和属性。

靠上述文档说明基本可以完成常见的图标控制.下面附上一条福利:

1、初始化时的图例控制.

查看API可知在方法和属性中找到series中有hide()即可.通过控制series数值或者name之类的即可 。

关于之前我说控制Y轴的消显(消失/显示)功能和导出列表控制这块问题估计近期也将有可能出现改善~敬请期待.