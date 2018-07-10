---
title: 图表太丑怎么破，ECharts神器带你飞！
date: 2017-10-10 14:28:57
categories: "开发"
tags:
	- 可视化
	- 张佳玮
	- 文章
	- 简书
	- 数据挖掘

---

# 一、前言 #

在本专栏或文集中，我曾多次使用ECharts绘制图表、进行可视化，也渐渐积累了**30多个实例**，本文对此前用过的所有图表和代码进行整理并分享，以给想绘制精美图表的人一点绵薄的帮助。其中全部实例已上传**ECharts3官网的个人主页**，如果觉得网页上一个个代码查看太麻烦，可以看评论区，去某号后台自取，全部代码和原图轻松到手，妈妈再也不用担心你的图丑破天际了，（逃）。

![图表太丑怎么破，ECharts神器带你飞！][ECharts]

一只“治愈”的猫（微博）

# 二、喵喵喵 #

# 2.1、bug/词云/韦恩图 #

所有实例均是参考官网的作品实现的，本人对JavaScript此外无过多涉猎，若有不规范的地方，请批评性看待。实例中涉及动态交互的部分可能会有莫名其妙的地方，因为此前都是为了拿到静态图表，如果你也只是想拿个图，那问题倒是不大。

**词云**不是ECharts绘制的，而是由下面的网站生成的，可选择多个主题、配色、形状等等。

在线词云生成网站：HTML5 Word Cloud（有点卡，布局也要多调试）

![图表太丑怎么破，ECharts神器带你飞！][ECharts 1]

**韦恩图**只能在ECharts2里运行成功，故没有搬到ECharts3里：

![图表太丑怎么破，ECharts神器带你飞！][ECharts 2]

# 2.2、JavaScript语法 #

**“//”：可注释掉一行代码**，类似python中的“\#”；

**“/\*....\*/”：可将中间多行、整块的代码注释掉**，类似python里的三引号（'''....'''）；

**toolbox：工具栏**。内置有**导出图片**（**saveAsImage**），数据视图（**dataView**），动态类型切换（**magicType**），数据区域缩放（**brush**），重置（**restore**）五个工具。均在图表的右上角，导出图片均添加了，其他不需要的可以用“//”注释掉。

其他内容，可参考官网“**配置项**”详细内容，新手可以对照简单的图表实例，一点点啃。

# 2.3、教程 #

**慕课网视频：《Echarts3.0入门基础与实战》**

**W3Cschool：《ECharts教程》**

以上教程都没怎么完整看过，还是推荐新手简单熟悉后，对照官网实例进行钻研。其中ECharts有2.0版和3.0版：ECharts2 官网：实例、ECharts3 官网：实例。其中，官网实例里图表很多，点开可能会卡，+10s 就好了......

# 三、实例 #

讲了这一车的话，对于不明真相的读者来说，可能最好奇的就是到底都有哪些图表呢，到底能把自己丑爆的图碾压到什么程度呢，以下有针对性的罗列下部分实例和对应的文章：

# 3.1、《爬取张佳玮138w+知乎关注者：数据可视化》 #

# 3.1.1 金字塔图 #

备注：张佳玮138万+知乎关注者的分布情况。

注意：设置series里的min和max为数据集的最小值和最大值。缺点是上几层文字显示不佳。

链接：http://gallery.echartsjs.com/editor.html?c=xB1Yh-iOhW

![图表太丑怎么破，ECharts神器带你飞！][ECharts 3]

# 3.1.2 柱形图和饼图（组合图） #

备注：张佳玮138万+知乎关注者之100+关注者的性别情况。

注意：**组合图只需在series下面多加几个“\{...\}”的数据集**，然后移动到合适位置进行展示即可。本实例原本是ECharts2里的，局部修改后也搬到ECharts3里的。此外每个实例均可**“切换主题”**，改变配色方案。

链接：http://gallery.echartsjs.com/editor.html?c=xBktHLn\_nZ&v=4

![图表太丑怎么破，ECharts神器带你飞！][ECharts 4]

# 3.1.3 分布地图 #

备注：张佳玮138万+知乎关注者之1w+关注者国内分布情况。

注意：绘制时需要**加载地图脚本**（/dep/echarts/map/js/china.js）。也需要将手头的城市数据转换成对应的经纬度。

链接：http://gallery.echartsjs.com/editor.html?c=xB1OhKsunW

![图表太丑怎么破，ECharts神器带你飞！][ECharts 5]

# 3.1.4 南丁格尔玫瑰图 #

备注：张佳玮138万+知乎关注者之Top20学校和行业情况。

注意：**南丁格尔玫瑰图有面积模式和半径模式**，具体区别有待专业人士指导，自己完全是那么好看用的那个。

链接：http://gallery.echartsjs.com/editor.html?c=xrJcYRounb

![图表太丑怎么破，ECharts神器带你飞！][ECharts 6]

备注：张佳玮138万+知乎关注者之Top20公司和职业情况。

链接：http://gallery.echartsjs.com/editor.html?c=xS1jD6od2-

![图表太丑怎么破，ECharts神器带你飞！][ECharts 7]

# 3.1.5 饼图 #

备注：张佳玮138万+知乎关注者之认证信息。

注意：**内圈和外圈数据要一致**。外圈作为内圈的细分。

![图表太丑怎么破，ECharts神器带你飞！][ECharts 8]

# 3.1.6 散点图 #

备注：张佳玮138万+知乎关注者之优秀回答者贡献情况。

![图表太丑怎么破，ECharts神器带你飞！][ECharts 9]

# 3.2、《爬取老树画画全部微博数据：三千诗与画》 #

# 3.2.1 日历热图（calendar heatmap） #

备注：老树画画历年发布微博情况。

链接：三年图：

http://gallery.echartsjs.com/editor.html?c=xrkp\_Jadnb

http://gallery.echartsjs.com/editor.html?c=xSkgCR3u3b

![图表太丑怎么破，ECharts神器带你飞！][ECharts 10]

一年图：http://gallery.echartsjs.com/editor.html?c=xBy6M16un-

![图表太丑怎么破，ECharts神器带你飞！][ECharts 11]

# 3.2.2 散点图 #

备注：老树画画微博之评论、转发与点赞情况。

注意：可与上面单组数据对照，看看两组数据时对应的代码异同之处。

链接：http://gallery.echartsjs.com/editor.html?c=xBy4c63d3Z

![图表太丑怎么破，ECharts神器带你飞！][ECharts 12]

# 3.3、《Gephi绘制微博转发图谱：以“@老婆孩子在天堂”为例》 #

# 3.3.1 柱形图 #

备注：一则微博转发数随时间变化情况。

链接：http://gallery.echartsjs.com/editor.html?c=xBJSSvnu2b&v=2

![图表太丑怎么破，ECharts神器带你飞！][ECharts 13]

# 3.4、《我的简书一月记：数据可视化》 #

# 3.4.1 气泡图 #

备注：我的简书一月记之点赞分布图。

注意：此处的时间为**时间戳**，而非一般的小时、分钟、秒数。**气泡大小代表点赞用户自身的粉丝数。**

链接：http://gallery.echartsjs.com/editor.html?c=xrJU9ceY2-

![图表太丑怎么破，ECharts神器带你飞！][ECharts 14]

备注：我的简书一月记之关注分布图。

注意：气泡大小统一。

链接：http://gallery.echartsjs.com/editor.html?c=xS1Kksgt3b

![图表太丑怎么破，ECharts神器带你飞！][ECharts 15]

# 3.4.2 单轴气泡图 #

备注：我的简书一月记之**20170827-20170829复盘**。

注意：20170828当天粉丝数猛然增长，单日涨粉170人次，同样的获赞数也增长了很多，于是复盘下前后三天的反馈信息，细分到每小时的情况。ECharts里还有事件流的图表，或许更合适这类复盘，但了解的还不错。

链接：http://gallery.echartsjs.com/editor.html?c=xSkk\_Yet2Z

![图表太丑怎么破，ECharts神器带你飞！][ECharts 16]

# 3.5、《简书=鸡汤？爬取今日看点数据：1916篇简书热门文章可视化》 #

# 3.5.1 柱形图和玫瑰图（组合图） #

备注：简书热门文章之年度月份分布情况。

注意：**相同年份设置为同一颜色**，颜色选择很关键。

链接：http://gallery.echartsjs.com/editor.html?c=xrk0EHWF2b

![图表太丑怎么破，ECharts神器带你飞！][ECharts 17]

# 3.6、《爬取简书26万+用户信息：数据可视化》 #

# 3.6.1 柱形图和饼图（组合图） #

备注：简书26万用户数据之ID分布情况。

注意：指定好各部分的颜色。否则容易混乱。

链接：http://gallery.echartsjs.com/editor.html?c=xHknhaxKh-&v=5

![图表太丑怎么破，ECharts神器带你飞！][ECharts 18]

# 3.6.2 瀑布图 #

备注：简书26万用户信息之粉丝分布情况。

注意：data里总数值和净数值。前者从0开始。

链接：http://gallery.echartsjs.com/editor.html?c=xSkUYCxK2b

![图表太丑怎么破，ECharts神器带你飞！][ECharts 19]

# 3.6.3 金字塔图和饼图（组合图） #

备注：简书26万用户信息之粉丝情况。

链接：http://gallery.echartsjs.com/editor.html?c=xS19m0et3W

![图表太丑怎么破，ECharts神器带你飞！][ECharts 20]

# 3.7 《我的简书两月记：数据可视化》 #

# 3.7.1 瀑布图 #

备注：我的简书两月记之获赞数变化情况。

注意：同上的瀑布图。

链接：http://gallery.echartsjs.com/editor.html?c=xr1BkabF3Z

![图表太丑怎么破，ECharts神器带你飞！][ECharts 21]

# 3.7.2 折线图（一） #

备注：我的简书两月记之粉丝数和获赞数每日变化情况。

链接：http://gallery.echartsjs.com/editor.html?c=xByBciWK3b

![图表太丑怎么破，ECharts神器带你飞！][ECharts 22]

# 3.7.3 折线图（二） #

备注：我的简书两月记之粉丝数每日变化情况。

注意：折线和横坐标没对齐，bug。

链接：http://gallery.echartsjs.com/editor.html?c=xBya6oZY3b

![图表太丑怎么破，ECharts神器带你飞！][ECharts 23]

# 3.7.4 折线图（三） #

备注：我的简书两月记之Top5 文章获赞情况。

链接：http://gallery.echartsjs.com/editor.html?c=xS12e1ft3-

![图表太丑怎么破，ECharts神器带你飞！][ECharts 24]

# 四、小结 #

以上所有实例均已上传至**ECharts3官网的个人主页**，如果觉得网页上一个个代码查看太麻烦，可以看评论区，去某号后台自取，全部代码和原图轻松到手，妈妈再也不用担心你的图丑破天际了（逃）。

ECharts3链接：

**http://gallery.echartsjs.com/explore.html?u=bd-3190370387&type=work\#sort=rank~timeframe=all~author=all**


[ECharts]: /pro/os/crawler/2IJ6-BMZY-ZABF.jpg
[ECharts 1]: /pro/os/crawler/7V3E-IVIA-MUUE.jpg
[ECharts 2]: /pro/os/crawler/NMVV-J3VB-UMZ3.jpg
[ECharts 3]: /pro/os/crawler/ZQVF-ZYRE-EZUU.jpg
[ECharts 4]: /pro/os/crawler/3UJA-AAFQ-VY73.jpg
[ECharts 5]: /pro/os/crawler/IMMI-UMQE-B3YF.jpg
[ECharts 6]: /pro/os/crawler/R3EY-VAAR-JZAU.jpg
[ECharts 7]: /pro/os/crawler/3YRY-BFNZ-ENA2.jpg
[ECharts 8]: /pro/os/crawler/EVBF-22UE-NYMN.jpg
[ECharts 9]: /pro/os/crawler/RZV2-EUIJ-QYIY.jpg
[ECharts 10]: /pro/os/crawler/N2UU-ANFA-JZZF.jpg
[ECharts 11]: /pro/os/crawler/RNNJ-FBIN-F3Y3.jpg
[ECharts 12]: /pro/os/crawler/VMYQ-FVUN-ZEJR.jpg
[ECharts 13]: /pro/os/crawler/VNF3-QAUJ-JIUI.jpg
[ECharts 14]: /pro/os/crawler/FAFY-V3RJ-QYMZ.jpg
[ECharts 15]: /pro/os/crawler/VJVV-BVMV-UU2Q.jpg
[ECharts 16]: /pro/os/crawler/IFVE-RR73-YVZI.jpg
[ECharts 17]: /pro/os/crawler/FIZA-FAM6-ZYA2.jpg
[ECharts 18]: /pro/os/crawler/NIVZ-QEIZ-U7RV.jpg
[ECharts 19]: /pro/os/crawler/U6NE-6V2A-Z7V3.jpg
[ECharts 20]: /pro/os/crawler/MABY-6NYU-YNJN.jpg
[ECharts 21]: /pro/os/crawler/EIZN-AR7N-EFMR.jpg
[ECharts 22]: /pro/os/crawler/2IJN-YNIN-ZMRR.jpg
[ECharts 23]: /pro/os/crawler/I77R-Q2VB-AYJI.jpg
[ECharts 24]: /pro/os/crawler/RZ63-YFVJ-QUBA.jpg
 *  **原文作者：** 爬虫和数据可视化
 *  **原文链接：** https://www.toutiao.com/item/6475161319304593933/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。