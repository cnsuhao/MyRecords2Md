---
title: Highchart导出列表的控制
date: 2014-02-18 18:02:43
categories: "开发"
tags:
	- chart

---

在原生Highcharst里.该图表库提供了一些监听事件.接口如下:

``````````
                    chart: {
``````````

renderTo: 'domId',

events: \{

addSeries: function() \{

//当添加series时被触发

\},

click :function()\{

//当点击导出的按钮时被触发

\},

load : function()\{

//图表加载数据时被触发

\},

redraw:function()\{

//当图表被重绘时触发

\}，

selection :function()\{

//图表被选中区域的事件

\}

//…上述监听回调里传递的参数(对象),请通过console.log(arguments);查看。

\}

\}

但这些里唯独不提供![ZRMV-JRIJ-YVZZ.jpg][]导出列表中的选中(click)的事件监听.另外我暂时还没尝试能否自定义列表的种类。

按照原生库里的做法.上图的列表汉化后分别是“打印曲线”、“保存png图片”、“保存jpeg图片”、“保存PDF文档”、“保存SCG矢量图”.

这里把jpeg更改为excel文件.是为了扩展其功能.

说完上述内容.我说下要怎么做：

当时的思路：

1、 尝试自定义里面的列表,发现其处理配置它是否启用以及汉化外,不再提供其他接口.

2、 我开始尝试在列表上下功夫.就是想控制其click事件,判断被点击的是什么.顺便把名字改了.好把原生的功能屏蔽掉,用于自己的功能控制.

当第一个方法没头绪时,开始尝试第二种.便发现原生的事件中仅仅提供了对导出按钮的控制.那么这种思路便卡住了。不过还好.既然有条路,一般都能往下走的.顺这这条路走下去.便找到了改按钮buttons属性,继续往下走.重要找到了列表内容!!于是便直接控制其dom.添加dom的onclick监听.同时发现其原生的onclick监听里所调用的方法.这里只是覆盖了JPEG的原生方法.供我控制.

在方法内部, fireEvent('exportExcel',scope,chart)代码部分.是触发了我自定义的一个事件,并传递了些对象供事件监听者调用。这里根据情况,看官可自定扩展。改内部代码我也没优化，请自行优化。

var menuItems = chart.options.exporting.buttons.contextButton.menuItems;

for(vari=0,len=menuItems.length;i<len;i++)\{

if(!menuItems\[i\])continue;

(function(menuItem,scope,chart)\{

if(menuItem.onclick)\{//分割线是没有事件的过滤

vartextKey = menuItem\['textKey'\];

if(textKey=="printChart")\{

menuItem.onclick= function()\{

this.print();

\}

\}elseif(textKey=='downloadPNG')\{

menuItem.onclick= function()\{

this.exportChart();

\}

\}elseif(textKey=='downloadJPEG')\{//把它原生的数据更改为导出excel！！！

menuItem.onclick= function()\{

//scope.fireEvent('exportExcel',scope,chart);

alert('导出excel文件功能敬请期待!');

//this.exportChart(\{type:'image/jpeg'\});

\}

\}elseif(textKey=='downloadPDF')\{

menuItem.onclick= function()\{

this.exportChart(\{type:'application/pdf'\});

\}

\}elseif(textKey=='downloadSVG')\{

menuItem.onclick= function()\{

this.exportChart(\{type:'image/svg+xml'\});

\}

\}

\}

\})(menuItems\[i\],me,chart);//分别代指:目录列表中的一个、作用域对象、图表对象

\}

说到这里,不得不吐槽下,为毛原生里不提供这里的监听呢? //为毛我还没发现如何自定义menuItems以及相应的处理？？貌似发现了！！此处屏蔽

说些后话:我们控制了相应的处理方法.那么对于导出而言,我们只需要将渲染改图形的数据得到,进而进行处理。处理方法我暂时提供两种思路:

1、 前端的导出(一般利用IE下的Active之类的或者类似我之前博文中提到的ext中grid导出excel方式).这需要内置一个接口来保存每次图表刷新后的数据.例如:

chart.\_setExportData = function(data)\{this.\_exportData= data;\}

chart.\_getExportData = function()\{returnthis.\_exportData;\}

这样的话可以根据图表实际的数据类型做相应的处理即可。

2、 在监听内获取渲染图表所使用的条件.根据图表渲染所使用的请求路径等条件.重新发送一条数据请求,在后台处理,并以文件流的方式下载到浏览器端。

两种思路里，建议第二种。除导出excel文件外,其他文件类型均可根据实际需求扩展(实际上原生的方法就是把这些发送到默认URL到后端处理里,只是我不清楚内部的处理在哪里,但可以肯定的是它本身肯定保存了数据。). as your free~

\--请尊重原创！如有转载请标注转载地址！

\--改文的资源同步上传到我的资源中,供免费下载


[ZRMV-JRIJ-YVZZ.jpg]: /pro/os/crawler/ZRMV-JRIJ-YVZZ.jpg