---
title: Jfreechart的样例copy(转)
date: 2012-12-18 21:04:14
categories: "开发"
tags:
	- chart

---

jfreechart目前最高版本为1.0.0版(http://www.jfree.org/jfreechart/index.html)。可以绘制
pie charts 饼图,bar charts 柱状图,line and area charts曲线图,scatter plots and bubble charts 散列图,time series 时序图,Area Charts区域图,Difference Chart差异图,Step Chart步骤图,Multiple Axis Charts 混合图,Gantt charts甘特图，combination charts 复合图
JFreeChart核心类库介绍：
jfreechart主要由两个大的包组成：org.jfree.chart,org.jfree.data。其中前者主要与图形
本身有关，后者与图形显示的数据有关。

核心类主要有：
org.jfree.chart.JFreeChart：图表对象，任何类型的图表的最终表现形式都是在该对象进行一些属性的定制。JFreeChart引擎本身提供了一个工厂类用于创建不同类型的图表对象
org.jfree.data.category.XXXDataSet:数据集对象，用于提供显示图表所用的数据。根据不同类型的图表对应着很多类型的数据集对象类
org.jfree.chart.plot.XXXPlot：图表区域对象，基本上这个对象决定着什么样式的图表，创建该对象的时候需要Axis、Renderer以及数据集对象的支持
org.jfree.chart.axis.XXXAxis：用于处理图表的两个轴：纵轴和横轴
org.jfree.chart.render.XXXRender：负责如何显示一个图表对象
org.jfree.chart.urls.XXXURLGenerator:用于生成Web图表中每个项目的鼠标点击链接
XXXXXToolTipGenerator:用于生成图象的帮助提示，不同类型图表对应不同类型的工具提示类
对于常用的饼图阖柱状图，比较简单而且网上有很多的文章介绍，在这里就不再一一复述了，
(可以参考这篇文章http://www-128.ibm.com/developerworks/cn/java/l-jfreechart/index.html?ca=dwcn-isc&me=ccid）
主要说明下另一种常见的报表，时序图，首先声明一个曲线数据集合对象和曲线对象


TimePeriodValuesCollection timeseriescollection = new TimePeriodValuesCollection();
//声明具体是曲线对象，(可根据实际情况在同一张图中显示多条曲线进行数据比对，根据实际应用情况当超过4条曲线时，就会有些乱。)
TimePeriodValues timeperiod1 = new TimePeriodValues("服务器A在线用户数量");
TimePeriodValues timeperiod2 = new TimePeriodValues("服务器B在线用户数量");
我在使用TimeSeriesCollection tsc = new TimeSeriesCollection();
TimeSeries ts = new TimeSeries();
在生成数据集时（ts.add(new Day(day, month, year),10))）只能生成最小单位为天的横轴所以改用了TimePeriodValuesCollection
//根据当前时间取得横轴坐标，时间间隔为1小时
Calendar cal = Calendar.getInstance();
int year = cal.get(Calendar.YEAR);
int month = cal.get(Calendar.MONTH) + 1;
int day = cal.get(Calendar.DAY\_OF\_MONTH);
//这里改为根据自己程序得到的需要显示的时间点和对应的数据的集合;
List objectList1 = dao.getList1();
List objectList2 = dao.getList2();
//使用循环，把x轴，y轴的值赋给timeseries1
for (int i =0;i<objecthash1.size();i++) \{
int hour = objecthash1\[i\].getHours();
int count = objecthash1\[i\].getCount();
//将每一对数据（时间，数值）添加到数据集合1(曲线对象1)中
timeseries1.add(new Hour(hour, day, month, year),count);
\}
for (int i =0;i<objecthash2.size();i++) \{
int hour = objecthash2\[i\].getHours();
int count = objecthash2\[i\].getCount();
//将每一对数据（时间，数值）添加到数据集合2(曲线对象2)中
timeseries2.add(new Hour(hour, day, month, year),count);
\}
//将曲线对象添加到曲线数据集合对象中
timeseriescollection.addSeries(timeseries1);
timeseriescollection.addSeries(timeseries2);
//绘制报表
String title = "日在线用户统计"; //报表标题
String domain = "时间"; //x轴
String range = "用户在线数量"; //y轴
//创建时间序列图对象
JFreeChart chart = ChartFactory.createTimeSeriesChart(
title, //报表标题
domain, //报表横轴标签
range, //报表纵轴标签
timeseriescollection, //数据集合
true, //是否显示图例,在这里如果为true则会在图表的下方显示各条数据曲线的名称和颜色
false, // 是否生成工具
false // 是否生成URL链接);
//将报表保存为jpg文件
ChartUtilities.saveChartAsJPEG(file, //文件保存物理路径包括路径和文件名
100, //图片质量
chart, //图表对象
1024, //图像宽度
768, //图像高度
null); //显示信息
//将报表直接在页面输出
ChartUtilities.writeChartAsJPEG(res.getOutputStream(),100,chart,1024,768,null);

String title="月在线用户统计"; //标题
String domain="时间(天)";//x轴
String range="用户在线数量";//y轴
TimePeriodValuesCollection timeseriescollection = new TimePeriodValuesCollection();
TimePeriodValues timeseries = new TimePeriodValues( "用户数量");
timeseries.add(new Minute(0, 1, 1, 1, 2006), 100);
timeseries.add(new Minute(10, 1, 1, 1, 2006), 500);
timeseries.add(new Minute(20, 1, 1, 1, 2006), 300);
timeseries.add(new Minute(30, 1, 1, 1, 2006), 800);
JFreeChart chart =ChartFactory.createTimeSeriesChart(title,domain,range,timeseriescollection,true,false,false);
当我们生成了一个报表对象时，可能需要根据实际情况来决定报表的横轴和纵轴的数值间隔，显示方式等。
可以用XYPlot xyplot = (XYPlot)chart.getPlot();来得到所有数据点的集合。（其它形状图表得到的数据集对象根据实际情况造型）
得到数据点集合后，我们就可以设置各条曲线的颜色，和坐标轴的距离，x轴、y轴的显示方式等等属性
xyplot.setBackgroundPaint(Color.lightGray); //设定图表数据显示部分背景色
xyplot.setAxisOffset(new RectangleInsets(5D, 5D, 5D, 5D)); //设定坐标轴与图表数据显示部分距离
xyplot.setDomainGridlinePaint(Color.white); //网格线纵向颜色
xyplot.setRangeGridlinePaint(Color.white); //网格线横向颜色

数据点的调整
XYLineAndShapeRenderer xylineandshaperenderer = (XYLineAndShapeRenderer)xyplot.getRenderer();
xylineandshaperenderer.setDefaultShapesVisible(true); //数据点可见
xylineandshaperenderer.setSeriesFillPaint(0, Color.red); //设置第一条曲线数据点填充为红色，如果一个图表有多条曲线可分别设置
xylineandshaperenderer.setUseFillPaint(true); //应用

使用xyplot.getRangeAxis()得到纵轴，xyplot.getDomainAxis()得到横轴，得到后可以根据实际情况造型为自己所需要的类型。
我的图表纵轴为数值类型，横轴为时间类型，使用如下方式
NumberAxis numAxis = (NumberAxis)xyplot.getRangeAxis();
DateAxis dateaxis = (DateAxis)xyplot.getDomainAxis();
//设置y显示方式
numAxis.setAutoTickUnitSelection(false);//数据轴的数据标签是否自动确定
double rangetick = 0.1D;
numAxis.setTickUnit(new NumberTickUnit(rangetick)); //y轴单位间隔为0.1
//设置x轴显示方式
dateaxis.setAutoTickUnitSelection(false);//数据轴的数据标签是否自动确定
dateaxis.setTickUnit(new DateTickUnit(DateTickUnit.DAY,1));//x轴单位间隔为1天
我们还可以是将数据格式化以后显示，比如y轴显示百分比（10%～100%）,x轴显示为×月×日
NumberFormat nf =NumberFormat.getPercentInstance();
numAxis.setNumberFormatOverride(nf);//设置y轴以百分比方式显示
SimpleDateFormat format = new SimpleDateFormat("MM月dd");
dateaxis.setDateFormatOverride(format);//设置x轴数据单位以×月×日方式显示
时序图中还有一个很重要的方法
timeseriescollection.setDomainIsPointsInTime(true); //x轴上的刻度点代表的是时间点而不是时间段
最开始我没有设置这个属性,结果画出来的图，老是差半格不能在这个刻度的时候准确显示，往后移了半格，就是因为JFreeChart默认这个刻度是
一个时间段，它把这个刻度和下个刻度的中间点认为是显示数据点最佳位置。

其他一些关于AXIS类的方法：

Axis类：
void setVisible(boolean flag)坐标轴是否可见
void setAxisLinePaint(Paint paint)坐标轴线条颜色（3D轴无效）
void setAxisLineStroke(Stroke stroke)坐标轴线条笔触（3D轴无效）
void setAxisLineVisible(boolean visible)坐标轴线条是否可见（3D轴无效）
void setFixedDimension(double dimension)（用于复合表中对多坐标轴的设置）
void setLabel(String label)坐标轴标题
void setLabelFont(Font font)坐标轴标题字体
void setLabelPaint(Paint paint)坐标轴标题颜色
void setLabelAngle(double angle)\`坐标轴标题旋转角度（纵坐标可以旋转）
void setTickLabelFont(Font font)坐标轴标尺值字体
void setTickLabelPaint(Paint paint)坐标轴标尺值颜色
void setTickLabelsVisible(boolean flag)坐标轴标尺值是否显示
void setTickMarkPaint(Paint paint)坐标轴标尺颜色
void setTickMarkStroke(Stroke stroke)坐标轴标尺笔触
void setTickMarksVisible(boolean flag)坐标轴标尺是否显示

ValueAxis(Axis)类：
void setAutoRange(boolean auto)自动设置数据轴数据范围
void setAutoRangeMinimumSize(double size)自动设置数据轴数据范围时数据范围的最小跨度
void setAutoTickUnitSelection(boolean flag)数据轴的数据标签是否自动确定（默认为true）
void setFixedAutoRange(double length)数据轴固定数据范围（设置100的话就是显示MAXVALUE到MAXVALUE-100那段数据范围）
void setInverted(boolean flag)数据轴是否反向（默认为false）
void setLowerMargin(double margin)数据轴下（左）边距
void setUpperMargin(double margin)数据轴上（右）边距
void setLowerBound(double min)数据轴上的显示最小值
void setUpperBound(double max)数据轴上的显示最大值
void setPositiveArrowVisible(boolean visible)是否显示正向箭头（3D轴无效）
void setNegativeArrowVisible(boolean visible)是否显示反向箭头（3D轴无效）
void setVerticalTickLabels(boolean flag)数据轴数据标签是否旋转到垂直
void setStandardTickUnits(TickUnitSource source)数据轴的数据标签（可以只显示整数标签，需要将AutoTickUnitSelection设false）

NumberAxis(ValueAxis)类：
void setAutoRangeIncludesZero(boolean flag)是否强制在自动选择的数据范围中包含0
void setAutoRangeStickyZero(boolean flag)是否强制在整个数据轴中包含0，即使0不在数据范围中
void setNumberFormatOverride(NumberFormat formatter)数据轴数据标签的显示格式
void setTickUnit(NumberTickUnit unit)数据轴的数据标签（需要将AutoTickUnitSelection设false）

DateAxis(ValueAxis)类：
void setMaximumDate(Date maximumDate)日期轴上的最小日期
void setMinimumDate(Date minimumDate)日期轴上的最大日期
void setRange(Date lower,Date upper)日期轴范围
void setDateFormatOverride(DateFormat formatter)日期轴日期标签的显示格式
void setTickUnit(DateTickUnit unit)日期轴的日期标签（需要将AutoTickUnitSelection设false）
void setTickMarkPosition(DateTickMarkPosition position)日期标签位置（参数常量在org.jfree.chart.axis.DateTickMarkPosition类中定义）

CategoryAxis(Axis)类：
void setCategoryMargin(double margin)分类轴边距
void setLowerMargin(double margin)分类轴下（左）边距
void setUpperMargin(double margin)分类轴上（右）边距
void setVerticalCategoryLabels(boolean flag)分类轴标题是否旋转到垂直
void setMaxCategoryLabelWidthRatio(float ratio)分类轴分类标签的最大宽度
jfreechart 设置技巧

1.横坐标内容竖立
XYPlot xyplot = jfreechart.getXYPlot();
DateAxis dateaxis = (DateAxis)xyplot.getDomainAxis();
dateaxis.setTickUnit(new DateTickUnit(1, 1, new SimpleDateFormat("MMM-yyyy")));
dateaxis.setVerticalTickLabels(true);

2.设置最大坐标范围
1）ValueAxis axis = xyplot.getRangeAxis() ;
axis.setRange(0,100) ;
xyplot.setRangeAxis(axis);

2）numberaxis1.setUpperBound(6500D);//最大值
numberaxis1.setLowerBound(5500D);//最小值
2.设置时间轴的间隔时间
dateaxis.setTickUnit(new DateTickUnit(DateTickUnit.DAY,1));//设置时间间隔为一天
学JFreeChart不得不看的中文API

JFreeChart类：
void setAntiAlias(boolean flag)字体模糊边界
void setBackgroundImage(Image image)背景图片
void setBackgroundImageAlignment(int alignment)背景图片对齐方式（参数常量在org.jfree.ui.Align类中定义）
void setBackgroundImageAlpha(float alpha)背景图片透明度（0.0～1.0）
void setBackgroundPaint(Paint paint)背景色
void setBorderPaint(Paint paint)边界线条颜色
void setBorderStroke(Stroke stroke)边界线条笔触
void setBorderVisible(boolean visible)边界线条是否可见

\-----------------------------------------------------------------------------------------------------------

TextTitle类：
void setFont(Font font)标题字体
void setPaint(Paint paint)标题字体颜色
void setText(String text)标题内容

\-----------------------------------------------------------------------------------------------------------

StandardLegend(Legend)类：
void setBackgroundPaint(Paint paint)图示背景色
void setTitle(String title)图示标题内容
void setTitleFont(Font font)图示标题字体
void setBoundingBoxArcWidth(int arcWidth)图示边界圆角宽
void setBoundingBoxArcHeight(int arcHeight)图示边界圆角高
void setOutlinePaint(Paint paint)图示边界线条颜色
void setOutlineStroke(Stroke stroke)图示边界线条笔触
void setDisplaySeriesLines(boolean flag)图示项是否显示横线（折线图有效）
void setDisplaySeriesShapes(boolean flag)图示项是否显示形状（折线图有效）
void setItemFont(Font font)图示项字体
void setItemPaint(Paint paint)图示项字体颜色
void setAnchor(int anchor)图示在图表中的显示位置（参数常量在Legend类中定义）

Axis类：
void setVisible(boolean flag)坐标轴是否可见
void setAxisLinePaint(Paint paint)坐标轴线条颜色（3D轴无效）
void setAxisLineStroke(Stroke stroke)坐标轴线条笔触（3D轴无效）
void setAxisLineVisible(boolean visible)坐标轴线条是否可见（3D轴无效）
void setFixedDimension(double dimension)（用于复合表中对多坐标轴的设置）
void setLabel(String label)坐标轴标题
void setLabelFont(Font font)坐标轴标题字体
void setLabelPaint(Paint paint)坐标轴标题颜色
void setLabelAngle(double angle)\`坐标轴标题旋转角度（纵坐标可以旋转）
void setTickLabelFont(Font font)坐标轴标尺值字体
void setTickLabelPaint(Paint paint)坐标轴标尺值颜色
void setTickLabelsVisible(boolean flag)坐标轴标尺值是否显示
void setTickMarkPaint(Paint paint)坐标轴标尺颜色
void setTickMarkStroke(Stroke stroke)坐标轴标尺笔触
void setTickMarksVisible(boolean flag)坐标轴标尺是否显示

ValueAxis(Axis)类：
void setAutoRange(boolean auto)自动设置数据轴数据范围
void setAutoRangeMinimumSize(double size)自动设置数据轴数据范围时数据范围的最小跨度
void setAutoTickUnitSelection(boolean flag)数据轴的数据标签是否自动确定（默认为true）
void setFixedAutoRange(double length)数据轴固定数据范围（设置100的话就是显示MAXVALUE到MAXVALUE-100那段数据范围）
void setInverted(boolean flag)数据轴是否反向（默认为false）
void setLowerMargin(double margin)数据轴下（左）边距
void setUpperMargin(double margin)数据轴上（右）边距
void setLowerBound(double min)数据轴上的显示最小值
void setUpperBound(double max)数据轴上的显示最大值
void setPositiveArrowVisible(boolean visible)是否显示正向箭头（3D轴无效）
void setNegativeArrowVisible(boolean visible)是否显示反向箭头（3D轴无效）
void setVerticalTickLabels(boolean flag)数据轴数据标签是否旋转到垂直
void setStandardTickUnits(TickUnitSource source)数据轴的数据标签（可以只显示整数标签，需要将AutoTickUnitSelection设false）

NumberAxis(ValueAxis)类：
void setAutoRangeIncludesZero(boolean flag)是否强制在自动选择的数据范围中包含0
void setAutoRangeStickyZero(boolean flag)是否强制在整个数据轴中包含0，即使0不在数据范围中
void setNumberFormatOverride(NumberFormat formatter)数据轴数据标签的显示格式
void setTickUnit(NumberTickUnit unit)数据轴的数据标签（需要将AutoTickUnitSelection设false）

DateAxis(ValueAxis)类：
void setMaximumDate(Date maximumDate)日期轴上的最小日期
void setMinimumDate(Date minimumDate)日期轴上的最大日期
void setRange(Date lower,Date upper)日期轴范围
void setDateFormatOverride(DateFormat formatter)日期轴日期标签的显示格式
void setTickUnit(DateTickUnit unit)日期轴的日期标签（需要将AutoTickUnitSelection设false）
void setTickMarkPosition(DateTickMarkPosition position)日期标签位置（参数常量在org.jfree.chart.axis.DateTickMarkPosition类中定义）

CategoryAxis(Axis)类：
void setCategoryMargin(double margin)分类轴边距
void setLowerMargin(double margin)分类轴下（左）边距
void setUpperMargin(double margin)分类轴上（右）边距
void setVerticalCategoryLabels(boolean flag)分类轴标题是否旋转到垂直
void setMaxCategoryLabelWidthRatio(float ratio)分类轴分类标签的最大宽度

AbstractRenderer类：
void setItemLabelAnchorOffset(double offset)数据标签的与数据点的偏移
void setItemLabelsVisible(boolean visible)数据标签是否可见
void setItemLabelFont(Font font)数据标签的字体
void setItemLabelPaint(Paint paint)数据标签的字体颜色
void setItemLabelPosition(ItemLabelPosition position)数据标签位置
void setPositiveItemLabelPosition(ItemLabelPosition position)正数标签位置
void setNegativeItemLabelPosition(ItemLabelPosition position)负数标签位置
void setOutLinePaint(Paint paint)图形边框的线条颜色
void setOutLineStroke(Stroke stroke)图形边框的线条笔触
void setPaint(Paint paint)所有分类图形的颜色
void setShape(Shape shape)所有分类图形的形状（如折线图的点）
void setStroke(Stroke stroke)所有分类图形的笔触（如折线图的线）
void setSeriesItemLabelsVisible(int series,boolean visible)指定分类的数据标签是否可见
void setSeriesItemLabelFont(int series,Font font)指定分类的数据标签的字体
void setSeriesItemLabelPaint(int series,Paint paint)指定分类的数据标签的字体颜色
void setSeriesItemLabelPosition(int series,ItemLabelPosition position)数据标签位置
void setSeriesPositiveItemLabelPosition(int series,ItemLabelPosition position)正数标签位置
void setSeriesNegativeItemLabelPosition(int series,ItemLabelPosition position)负数标签位置
void setSeriesOutLinePaint(int series,Paint paint)指定分类的图形边框的线条颜色
void setSeriesOutLineStroke(int series,Stroke stroke)指定分类的图形边框的线条笔触
void setSeriesPaint(int series,Paint paint)指定分类图形的颜色
void setSeriesShape(int series,Shape shape)指定分类图形的形状（如折线图的点）
void setSeriesStroke(int series,Stroke stroke)指定分类图形的笔触（如折线图的线）

AbstractCategoryItemRenderer(AbstractRenderer)类：
void setLabelGenerator(CategoryLabelGenerator generator)数据标签的格式
void setToolTipGenerator(CategoryToolTipGenerator generator)MAP中鼠标移上的显示格式
void setItemURLGenerator(CategoryURLGenerator generator)MAP中钻取链接格式
void setSeriesLabelGenerator(int series,CategoryLabelGenerator generator)指定分类的数据标签的格式
void setSeriesToolTipGenerator(int series,CategoryToolTipGenerator generator)指定分类的MAP中鼠标移上的显示格式
void setSeriesItemURLGenerator(int series,CategoryURLGenerator generator)指定分类的MAP中钻取链接格式

BarRenderer(AbstractCategoryItemRenderer)类：
void setDrawBarOutline(boolean draw)是否画图形边框
void setItemMargin(double percent)每个BAR之间的间隔
void setMaxBarWidth(double percent)每个BAR的最大宽度
void setMinimumBarLength(double min)最短的BAR长度，避免数值太小而显示不出
void setPositiveItemLabelPositionFallback(ItemLabelPosition position)无法在BAR中显示的正数标签位置
void setNegativeItemLabelPositionFallback(ItemLabelPosition position)无法在BAR中显示的负数标签位置

BarRenderer3D(BarRenderer)类：
void setWallPaint(Paint paint)3D坐标轴的墙体颜色

StackedBarRenderer(BarRenderer)类：
没有特殊的设置

StackedBarRenderer3D(BarRenderer3D)类：
没有特殊的设置

GroupedStackedBarRenderer(StackedBarRenderer)类：
void setSeriesToGroupMap(KeyToGroupMap map)将分类自由的映射成若干个组（KeyToGroupMap.mapKeyToGroup(series,group)）

LayeredBarRenderer(BarRenderer)类：
void setSeriesBarWidth(int series,double width)设定每个分类的宽度（注意设置不要使某分类被覆盖）

WaterfallBarRenderer(BarRenderer)类：
void setFirstBarPaint(Paint paint)第一个柱图的颜色
void setLastBarPaint(Paint paint)最后一个柱图的颜色
void setPositiveBarPaint(Paint paint)正值柱图的颜色
void setNegativeBarPaint(Paint paint)负值柱图的颜色

IntervalBarRenderer(BarRenderer)类：
需要传IntervalCategoryDataset作为数据源

GanttBarRenderer(IntervalBarRenderer)类：
void setCompletePaint(Paint paint)完成进度颜色
void setIncompletePaint(Paint paint)未完成进度颜色
void setStartPercent(double percent)设置进度条在整条中的起始位置（0.0～1.0）
void setEndPercent(double percent)设置进度条在整条中的结束位置（0.0～1.0）

StatisticBarRenderer(BarRenderer)类：
需要传StatisticCategoryDataset作为数据源

LineAndShapeRenderer(AbstractCategoryItemRenderer)类：
void setDrawLines(boolean draw)是否折线的数据点之间用线连
void setDrawShapes(boolean draw)是否折线的数据点根据分类使用不同的形状
void setShapesFilled(boolean filled)所有分类是否填充数据点图形
void setSeriesShapesFilled(int series,boolean filled)指定分类是否填充数据点图形
void setUseFillPaintForShapeOutline(boolean use)指定是否填充数据点的Paint也被用于画数据点形状的边框

LevelRenderer(AbstractCategoryItemRenderer)类：
void setItemMargin(double percent)每个分类之间的间隔
void setMaxItemWidth(double percent)每个分类的最大宽度

CategoryStepRenderer(AbstractCategoryItemRenderer)类：
void setStagger(boolean shouldStagger)不同分类的图是否交错

MinMaxCategoryRenderer(AbstractCategoryItemRenderer)类：
void setDrawLines(boolean drawLines)是否在每个分类线间画连接线
void setGroupPaint(Paint groupPaint)一组图形连接线的颜色
void setGroupStroke(Stroke groupStroke)一组图形连接线的笔触
void setMaxIcon(Icon maxIcon)最大值的ICON
void setMinIcon(Icon minIcon)最小值的ICON
void setObjectIcon(Icon objectIcon)所有值的ICON

AreaRender(AbstractCategoryItemRenderer)类：
没有特殊的设置

StackedAreaRender(AreaRender)类：
没有特殊的设置


关键就是用好Renderer这个类了，再贴个例子：
String sFont = "宋体";
chart.setBorderVisible(true);
chart.setBorderPaint(new Color(0xFF,0x66,0x00));
chart.setBackgroundPaint(new Color(0xFF,0xF3,0xDE));
chart.getTitle().setPaint(Color.red);
chart.getTitle().setFont(new Font(sFont,Font.BOLD,14));

//设置Plot，不显示所有网格
((CategoryPlot)chart.getPlot()).setOutlinePaint(null);
((CategoryPlot)chart.getPlot()).setDomainGridlinesVisible(false);
((CategoryPlot)chart.getPlot()).setRangeGridlinesVisible(false);

//设置横轴字体，设定横轴轴线不可见，隐藏纵轴
((CategoryPlot)chart.getPlot()).getDomainAxis().setTickLabelFont(new Font(sFont,Font.PLAIN,12));
((CategoryPlot)chart.getPlot()).getDomainAxis().setAxisLineVisible(false);
((CategoryPlot)chart.getPlot()).getRangeAxis().setVisible(false);

//采用BarRenderer作为表示器
BarRenderer renderer = new BarRenderer();
renderer.setPaint(new GradientPaint(0.0f,0.0f,Color.orange,0.0f,0.0f,Color.yellow));
renderer.setOutlinePaint(Color.orange);
renderer.setDrawBarOutline(true);

//在条中央显示投票数值
renderer.setItemLabelAnchorOffset(-20.0f);
renderer.setLabelGenerator(new StandardCategoryLabelGenerator("\{2\}",new DecimalFormat()));
renderer.setPositiveItemLabelPosition(new ItemLabelPosition());
renderer.setItemLabelsVisible(true);

我的需求是这样，需要做一个网站访问流量的变化趋势图，横坐标是时间，纵坐标是访问量即可。

刚开始我用linechart（就是折线图，可以带点，不带点）来做的，因为没有经验觉得linechart够用了，结果上线用了一个多月，随着时间的延长，我发线linechart图不够好了，因为时间长了之后linechart图并不能显示出来所有的数据点，只能显示出来35个左右的点位。假如我以天为时间单位，我要得到两个月，即60天的趋势图，那么它是显示不全的，数据点位会丢失，而且label也会变成“……”。

private static DefaultCategoryDataset createDefaultDataset()\{
String series1 = "First";
String series2 = "Second";
String series3 = "Third";

// column keys...
String type1 = "Type 1";
String type2 = "Type 2";
String type3 = "Type 3";
String type4 = "Type 4";
String type5 = "Type 5";
String type6 = "Type 6";
String type7 = "Type 7";
String type8 = "Type 8";

// create the dataset...
DefaultCategoryDataset dataset = new DefaultCategoryDataset();

dataset.addValue(1.0, series1, type1);
dataset.addValue(4.0, series1, type2);
dataset.addValue(3.0, series1, type3);
dataset.addValue(5.0, series1, type4);
dataset.addValue(5.0, series1, type5);
dataset.addValue(7.0, series1, type6);
dataset.addValue(7.0, series1, type7);
dataset.addValue(8.0, series1, type8);

dataset.addValue(5.0, series2, type1);
dataset.addValue(7.0, series2, type2);
dataset.addValue(6.0, series2, type3);
dataset.addValue(8.0, series2, type4);
dataset.addValue(4.0, series2, type5);
dataset.addValue(4.0, series2, type6);
dataset.addValue(2.0, series2, type7);
dataset.addValue(1.0, series2, type8);

dataset.addValue(4.0, series3, type1);
dataset.addValue(3.0, series3, type2);
dataset.addValue(2.0, series3, type3);
dataset.addValue(3.0, series3, type4);
dataset.addValue(6.0, series3, type5);
dataset.addValue(3.0, series3, type6);
dataset.addValue(4.0, series3, type7);
dataset.addValue(3.0, series3, type8);
return dataset;
\}
private static XYDataset createDataset()
\{
XYSeries xyseries = new XYSeries("allyes"); //先产生XYSeries 对象
xyseries.add(20070801, 220000);
xyseries.add(20070802, 210000);
xyseries.add(20070803, 250000D);
xyseries.add(20070804, 450000D);
xyseries.add(20070805, 270000D);
xyseries.add(20070806, 280000D);
xyseries.add(20070807, 290000D);
xyseries.add(20070808, 500000D);

XYSeries xyseries1 = new XYSeries("direct");
xyseries1.add(20070801, 230000D);
xyseries1.add(20070802, 240000D);
xyseries1.add(20070803, 250000D);
xyseries1.add(20070804, 260000D);
xyseries1.add(20070805, 270000D);
xyseries1.add(20070806, 480000D);
xyseries1.add(20070807, 290000D);
xyseries1.add(20070808, 250000D);

XYSeries xyseries2 = new XYSeries("iplus");
xyseries2.add(20070801, 240000D);
xyseries2.add(20070802, 280000D);
xyseries2.add(20070803, 270000D);
xyseries2.add(20070804, 290000D);
xyseries2.add(20070805, 230000D);
xyseries2.add(20070806, 310000D);
xyseries2.add(20070807, 400000D);
xyseries2.add(20070808, 220000D);

XYSeriesCollection xyseriescollection = new XYSeriesCollection(); //再用XYSeriesCollection添加入XYSeries 对象
xyseriescollection.addSeries(xyseries);
xyseriescollection.addSeries(xyseries1);
xyseriescollection.addSeries(xyseries2);
return xyseriescollection;
\}

JFreeChart jfreechart = ChartFactory.createXYLineChart(
des, x, y, dataset,//这里的dateset可以是适用于linechart的所有dataset集合，可以是DefaultCategoryDataset,XYDataset等等。前面给出了两个得到dataset集合的方法。
PlotOrientation.VERTICAL, true, true, false);
//外框背景色
jfreechart.setBackgroundPaint(Color.WHITE);

CategoryPlot categoryplot = (CategoryPlot) jfreechart.getPlot(); // 获得 plot

categoryplot.setBackgroundPaint(Color.BLACK); // 设定图表数据显示部分背景色
categoryplot.setOutlinePaint(Color.RED);//设置数据区的边界线条颜色
categoryplot.setRangeGridlinePaint(Color.BLACK);
categoryplot.setDomainAxisLocation(AxisLocation.TOP\_OR\_LEFT);
categoryplot.setAxisOffset(new RectangleInsets(5D, 5D, 5D, 5D)); // 设定坐标轴与图表数据显示部分距离
categoryplot.setDomainGridlinePaint(Color.white); // 网格线颜色
categoryplot.setRangeGridlinePaint(Color.white);

// 获得 renderer 注意这里是XYLineAndShapeRenderer ！！
LineAndShapeRenderer lineandshaperenderer = (LineAndShapeRenderer) categoryplot
.getRenderer();
lineandshaperenderer.setShapesVisible(false); // series 点（即数据点）可见
//设置线条宽度
//lineandshaperenderer.setSeriesStroke(0,new BasicStroke(5));
lineandshaperenderer.setStroke(new BasicStroke(4));

lineandshaperenderer.setSeriesStroke(0, new BasicStroke(2.0F, 1, 1,
1.0F, new float\[\] \{ 10F, 6F \}, 0.0F));
// 定义series为"First"的（即series1）点之间的连线 ，这里是虚线，默认是直线
lineandshaperenderer.setSeriesStroke(1, new BasicStroke(2.0F, 1, 1,
1.0F, new float\[\] \{ 6F, 6F \}, 0.0F));
// 定义series为"Second"的（即series2）点之间的连线
lineandshaperenderer.setSeriesStroke(2, new BasicStroke(2.0F, 1, 1,
1.0F, new float\[\] \{ 2.0F, 6F \}, 0.0F));
// 定义series为"Third"的（即series3）点之间的连线

lineandshaperenderer.setShapesFilled(true); // 数据点被填充即不是空心点

lineandshaperenderer.setStroke(new BasicStroke(4));//这个是设置线条的粗细

这些问题困扰着我，找了一些文档也没有解决。最后我改为了时间序列图TimeSeriesChart,它也是折线图，有横轴和纵轴，它的好处是，横轴的范围很大，不会丢失数据点位，而且可以按你想要的时间单位来区分间隔，很好用。如果是要做跟时间相关的，比如说一个时间对应一个数据值那就最好用这个chart。

private static TimeSeriesCollection createTimeseriesDataset(Map datasetmap) \{//假如你的数据存在了一个map里面
//最小单位时间是天
TimeSeriesCollection timeseriescollection = new TimeSeriesCollection();
//timeseriescollection.setDomainIsPointsInTime(false);
//最小单位时间自定义
//TimePeriodValuesCollection timeseriescollection = new TimePeriodValuesCollection();
TimeSeries ts = null;
HashMap returnMap = (HashMap)TestXML.sortStringMapbyKeyASC(datasetmap);
HashMap o\_map = new HashMap();
Iterator iterator = returnMap.entrySet().iterator();
while(iterator.hasNext())\{
Map.Entry entry = (Map.Entry)iterator.next();
String key =String.valueOf(entry.getKey().toString());
String value =String.valueOf(entry.getValue().toString());
ts.add(new Day(key),value);
\}
Iterator iter = o\_map.keySet().iterator();
while(iter.hasNext())\{
timeseriescollection.addSeries((TimeSeries)o\_map.get(iter.next().toString()));
\}
return timeseriescollection;
\}

\----------------------------------------------------------------------------------------

个人感觉jfreechart做静态图确实不错的，但是交互对统计来说也是很有必要的,jfreechart的交互就不是很好了（个人感觉）。我还是想把它利用在批量生成图片上来比较实用～fusionchart的效果和功能要比它好多了～但是fusionchart却不提供后台的生成图片的支持。所以两个结合着用还可以吧～


