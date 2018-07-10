---
title: 前端开发 一个简单、响应迅速、无依赖的现代SVG图表（Charts）
date: 2017-12-04 10:12:49
categories: "开发"
tags:
	- 技术
	- GitHub
	- HTML

---

> Charts是一个开源（MIT协议）的图表库，使用简单、响应迅速，拥有线型、柱型、散点型、饼图、百分比简单的图表类型，满足你对图表的基本需求，压缩包仅15kB，是一个非常精简且轻量级的图表。

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts]

1、快速开始


**https://github.com/frappe/charts**

在github上下载项目，然后解压压缩文件，把dist文件夹下的文件放到你项目的任意位置，在你的HTML页面中引入frappe-charts.min.iife.js文件。


![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 1]

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 2]

然后运行如下所示脚本代码，一个简单的柱状图表就生成了。

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 3]

data变量是图表的数据，可以通过ajax动态获取生成图表。

通过new Chart(options)方法实例化：

 *  parent：值可以是一个选择器，也可以是一个DOM元素。
 *  type：图表类型，有'bar','line','scatter','pie','percentage'五个类型。
 *  colors：颜色，如果不设置，将使用默认排列顺序。\['light-blue', 'blue', 'violet', 'red', 'orange', 'yellow', 'green', 'light-green', 'purple', 'magenta', 'grey', 'dark-grey'\]。
 *  format\_tooltip\_x、format\_tooltip\_y：分别是图表x轴和y轴的工具提示格式化方法。

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 4]

2、其他类型图表


 *  线型

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 5]

 *  散点型

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 6]

 *  饼图

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 7]

 *  百分比图
    

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 8]

3、动态更新数值


![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 9]

 *  chart.update\_values(datas,labels)：更新全部数值。
 *  chart.add\_data\_point(data,label,index)：指定位置添加一个数值点，默认添加到最后。
 *  remove\_data\_point(index)：移除一个指定位置数值点。

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 10]

4、简单的聚合


![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 11]

目前只有简单的总数和平均数两个聚合数据。

 *  chart.show\_sums()和chart.hide\_sums()：显示、隐藏总数。
 *  chart.show\_averages()和chart.hide\_averages()：显示、隐藏平均数。

5、监听事件

![前端开发：一个简单、响应迅速、无依赖的现代SVG图表（Charts）][SVG_Charts 12]

> chart.parent.addEventListener('data-select', function(e)\{console.log(e);\});

监听数据选择事件，可以用来动态展示点击数据的详情。

--------------------

这是一个简单的图标库，特点在于轻量级、加载速度快，如果你不需要Echarts那么丰富的图表，只需要一点简单的可以考虑一下Charts。

如果你有更好的、实用的插件想要和我们分享，请评论告诉我，感谢您的分享。


[SVG_Charts]: /pro/os/crawler/MQQ3-MFBE-INZM.jpg
[SVG_Charts 1]: /pro/os/crawler/YIUM-2MIJ-6JJI.jpg
[SVG_Charts 2]: /pro/os/crawler/FIIF-QQZE-MRJQ.jpg
[SVG_Charts 3]: /pro/os/crawler/2MJA-VNRZ-ZAIB.jpg
[SVG_Charts 4]: /pro/os/crawler/YVNE-A3QU-MYUQ.jpg
[SVG_Charts 5]: /pro/os/crawler/E6Z6-BNRY-ANYM.jpg
[SVG_Charts 6]: /pro/os/crawler/UN6Z-ZMNY-RQJJ.jpg
[SVG_Charts 7]: /pro/os/crawler/YUJR-3MRJ-MB7J.jpg
[SVG_Charts 8]: /pro/os/crawler/V6FF-Y26N-MFVU.jpg
[SVG_Charts 9]: /pro/os/crawler/F6NN-6RUY-6V7R.jpg
[SVG_Charts 10]: /pro/os/crawler/AM22-IJEV-JUJU.gif
[SVG_Charts 11]: /pro/os/crawler/FZFE-NMFN-6FJY.jpg
[SVG_Charts 12]: /pro/os/crawler/QFQA-MRZM-ZAN2.gif
 *  **原文作者：** IT痕迹
 *  **原文链接：** https://www.toutiao.com/item/6495506114535227917/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。