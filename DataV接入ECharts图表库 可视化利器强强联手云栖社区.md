---
title: DataV接入ECharts图表库 可视化利器强强联手
date: 2017-06-20 14:53:20
categories: "开发"
tags:
	- 可视化
	- 鼠标
	- JSON
	- 玫瑰
	- 数据挖掘

---

DataV 数据可视化是搭建每年天猫双十一作战大屏的幕后功臣，ECharts 是广受数据可视化从业者推崇的开源图表库。从今天开始，DataV 企业版接入了 ECharts 图表组件，当你使用 DataV 搭建可视化项目时，可以轻松地插入 ECharts，这意味着更丰富多样的图表效果，也让编程小白们可以通过图形界面而非代码配置 ECharts。

DataV 首批接入的 ECharts 图表总共有15种。囊括了关系、柱形、热力图、仪表盘等各种常用图形。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_]

比如大家都很熟悉的玫瑰图，

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 1]

还有日历图，

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 2]

炫酷的关系图，

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 3]

以及...K 线图，现在都可以在 DataV 里实现啦。有朋友问，你们为什么放个 K 线图进来？我当然不会告诉你是因为主力开发同学最近迷上炒股了啊。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 4]

接下来一起看看，加上了 ECharts 组件的 DataV 要怎么玩。

闪电式组件添加

首先，来做下准备工作。进入 DataV 任意可视化项目的编辑面板，点击上方菜单栏的“更多组件”按钮，选中“ECharts”图标，再点击确认添加，即可成功导入。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 5]

接下来这些 ECharts 图表就会出现在组件列表里面，使用方法和其他组件完全相同。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 6]

举个栗子，我们给上市公司“山水庄园”的看板上，加一张股价 K 线图，通过点击—拖动—调整大小三步实现，是不是顿时显得高大上。还能通过时间轴拖动和鼠标 hover 实现简单交互，这样高小琴靠着一张图就能掌控公司业务了。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 7]

图形化组件配置

我在开头提到，DataV 一大使用特点在于图形化界面配置，这对不熟悉代码、没时间学习 ECharts 文档的用户来说，绝对是福音。和原生组件一样，用户在使用的时候，在右侧面板中，可以完成对样式和数据源的配置。

比方说我们想修改这个漏斗图，先点击右侧控制面板的“数据”，选择接入的数据源，DataV 支持接入 JSON、CSV、数据库、API 等多种类型，完成数据上传并匹配字段后，再点击“样式”按钮，切换到样式编辑器，在这里调整图表颜色、字体、排列顺序、图例等元素即可。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 8]

经过封装后，原 ECharts 图表的配置项，在“样式”面板里全部保留，用户通过简单的下拉菜单和填写数字就能修改配置，所见即所得，学习成本大大降低。

与原生图表联动

讲完基础使用方法后，再来看看老司机们怎么玩。其实 DataV 原生图表库本身在样式和功能上都不错，和 ECharts 图表结合能做出不少特效。例如将时间轴和 ECharts 热力图结合，利用组件间的回调ID功能，就能得到动态热力图啦。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 9]

这里的关键步骤是在时间轴组件的回调ID中填入热力图数据源中的变量（该示例中即为年份），关于回调 ID 详细的操作方法可以参考这篇教程。掌握这个方法以后，就不难实现其他组件之间的交互效果了。

结语

以上洋洋洒洒介绍了这么多，相信大家都学会了，如果喜欢的话赶紧去 DataV 里试试吧。不过上面这些都不是我真正想说的，作为一个资深腐女，其实我眼中只看到了一对耀眼的，可视化当红 CP。

![DataV接入ECharts图表库 可视化利器强强联手][DataV_ECharts_ 10]


[DataV_ECharts_]: /pro/os/crawler/6RFB-EENN-BM6V.jpg
[DataV_ECharts_ 1]: /pro/os/crawler/AEQ7-BUAI-FJBV.gif
[DataV_ECharts_ 2]: /pro/os/crawler/NBQV-IFRJ-2Y3E.gif
[DataV_ECharts_ 3]: /pro/os/crawler/6FIV-AEUA-EJVU.gif
[DataV_ECharts_ 4]: /pro/os/crawler/VIIR-JBYB-RIEA.gif
[DataV_ECharts_ 5]: /pro/os/crawler/NYFB-VFQI-IEUJ.jpg
[DataV_ECharts_ 6]: /pro/os/crawler/V2EB-NAVI-3YUQ.jpg
[DataV_ECharts_ 7]: /pro/os/crawler/FFYZ-EJUI-6VJ3.gif
[DataV_ECharts_ 8]: /pro/os/crawler/Q26V-QUFF-VYYB.gif
[DataV_ECharts_ 9]: /pro/os/crawler/RJEJ-3ENM-J2UQ.gif
[DataV_ECharts_ 10]: /pro/os/crawler/63YM-VJN2-IAI3.jpg
 *  **原文作者：** 云栖社区
 *  **原文链接：** https://www.toutiao.com/item/6433610182214812162/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。