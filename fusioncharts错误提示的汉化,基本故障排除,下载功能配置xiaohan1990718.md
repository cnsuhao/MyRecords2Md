---
title: fusioncharts错误提示的汉化,基本故障排除,下载功能配置
date: 2012-12-20 12:02:47
categories: "开发"
tags:
	- chart

---

在使用fusioncharts的时候，有时候你可能要修改其默认的提示信息。如在默认情况下没有数据时显示:no data to display,你要实现自定义你可以通过以下方式实现。

**fusioncharts3.1中文提示解决**
var myChart = new FusionCharts("../FusionCharts/Column2D.swf?ChartNoDataText=无数据显示", "myChartId", "600","300");

**fusioncharts3.2中文提示解决**
<script type="text/javascript"><!--
var myChart = new FusionCharts("Column2D.swf","myChartId", "300", "250", "0","1");
myChart.setXMLUrl("<chart></chart>");
myChart.configure("ChartNoDataText", "Please select a record above");
 myChart.configure( "InvalidXMLText","Please validate data");
myChart.render("chartContainer");
// --></script>
3.2的解决方式较好，在3.1的fusioncharts.js中是没有configure这个方法的。此方法出在3.2中倒是有。不足的是现在网上3.2的破解版还不十分完美,swf文件差异较大.
详见官方文档:http://www.fusioncharts.com/docs/ChartMessages.html

\--------------------------------------------------------------------------------------

当试图使图Fusioncharts创建图表/仪表时，如果发现任何错误（或如果图表不能渲染），这可能是由于几个原因。在这里，我们会努力将它们排除掉。我们把整个调试过程分为三个部分：

**基本的故障排除——在这一节我们将讨论各种手工解决问题的方法。**

 **使用[Debug Mode][]（调试模式）——Fusioncharts和Fusionwidgets提供了一个调试窗口，你可以检测图表/仪表出现问题的原因。在这一节我们将告诉你如何使用调试窗口。**

 **使用[FCErrorEvent][] ——FusionCharts for Flex组件发生错误时，一定由一个事件引发。。本节重点放在如何处理事件，并确定错误。**

**让我们从基本的故障排除开始。当创建图表/仪表，如果因为某些原因你没看到您的图表/仪表该有的摸样，检查下列错误：**

**无法下载SWF Movie或者没有图表显示**

当浏览器中查看图表/仪表时，如果你发现图表/仪表下载过程永无休止，或如果右击菜单（右键点击图表/仪表应该出现的位置）显示'Movie not loaded'，检查以下问题：

检查在您的MXML代码中的SWF文件的路径是否正确。此外，检查SWF文件实际上存在于指定位置。

如果你使用的操作系统，它有一个区分大小写的文件系统，检查Case的路径和SWF文件。

检查您的计算机上是否有安装Adobe Flash Player 99（或以上）。

检查一下你是否已经启用浏览器中显示JavaScript / ActiveX控件。通常，所有浏览器都支持Flash和JavaScript。

出现"Error in Loading Data"信息

如果在你的图表/仪表看到"Error in Loading Data"消息，表示Fusioncharts在指定的网址找不着XML数据。如果是这样的话，检查以下：

检查您是否提供了FCDataURL正确的路径。

如果你使用 FCDataURL方法，在您的浏览器粘贴该网址，检查它是否是返回一个有效的XML。确保没有脚本或超时错误，并且返回一个有效的XML。同时确认没有混淆XML跟HTML。数据提供页面只会返回完全的XML数据——甚至没有HTML <head> 或者<body>标签。

如果你要从Fusioncharts传送参数到您的数据提供页面，检查他们在FCDataURL是URL编码的。例如，如果你的 FCDataURL是Data.asp?id=43&subId=454，你需要网址编码它，使它成为Data%2Easp%3Fid%3D43%26subId%3D454。这样Fusioncharts才能调用URL，与适当的参数附加到它。

当使用FCDataURL方法，请确认SWF文件和数据提供页面在同一个子域上。由于Flash的sandbox安全模型，它无法访问从外部域传来数据，除非有特殊的配置。

"Invalid XML Data" 信息

如果您看到一个"Invalid XML Data" 信息，这意味着XML数据文件有问题。再次检查相同的错误，比如：

不同的情况下标签。<chart> 应该以 </chart>结尾，而不是</Chart> 或者 </CHART> 。

缺少任何属性的打开/关闭引号。例如，<chart caption=Monthly Sales'应该是 <chart caption='Monthly Sales'

所有元素缺少关闭标签。

如果你使用 FCdataXML 方法，检查用于表示XML属性的字符和用于表示HTML属性之间的冲突。例如，如果您使用的是直接HTML嵌入方法，并附有HTML参数与一对双引号（“”），那么你必须附上所有属性在单引号（‘’）内。例子：<param name="FlashVars" value="<chart showLabels='1' showValues='1'>... </chart>" />

如果你有引号作为数据的一部分，XML彪马它们为 &apos;例如：<set name='John&apos;s House' />

如果你仍然无法在XML代码中找到错误。你也可以使用Debug Window（调试窗口）（下面介绍）或在您的浏览器打开XML，并手动检查错误。

"No data to display" 信息

如果你的图表显示"No data to display"消息，这可能是由于下列原因：

你的XML数据不包含任何数据，可用于绘制图表/仪表。在这种情况下，您的XML只包含<chart>或<dataset>标签，而没有数据包含在里面。

如果你使用单数列图表SWF，并在多级数列格式或者vice-versa提供数据。您会收到"No data to display"消息。

在一些双Y组合图表中，您需要提供至少一个数据集作为轴。否则，你会得到一个"No data to display"消息。

在下一节我们会告诉你如何使用调试窗口排除故障。

默认数据未根据上传的图表显示

之前FusionCharts for Flex v1.1的版本，在没有提供数据给图表时，图表用于显示图标一个默认的样本虚拟数据时。

从现在开始，图表不再显示默认的数据，如果默认的数据相关性能没有指定的话。您可以选择显示默认的数据，通过正确设置FusionCharts/FusionWidgets组件的 FCUseDefaultData 属性。默认地， FCUseDefaultData 属性设置为false，您的图表/仪表盘将显示"No data to display" 信息，如果您不提供任何数据。你还是可以同时使用FCUseDefaultData (true)和FCChartType 属性，来显示不同图表类型的默认的数据（样本虚拟数据）。

Flex API 不能正常工作

当要求由Flex API提供的多种方法，如果因为某种原因，他们没有工作，这可以根据以下原因确定：

检查您是否已经启用浏览器JavaScript/ActiveX控件。通常，所有浏览器都支持Flash和JavaScript。

Check whether you've enabled your browser to show JavaScript / ActiveX controls. Normally, all browsers are Flash-enabled and JavaScript enabled.

确保Flash播放器整体的安全设置配置正确。你可以在[Setting Flash Security Policies][]章节中学习设置政策的政策thesetting闪存安全政策。

为了让Image/PDF导出功能正常，确保应用程序在Flex SDK 3.3 for Flash Player 10中编译。

\------------------------------------------------------------------------------------------

![QNV7-FII3-YAZE.jpg][]

百闻不如一见，狠狠点击，快快下载：（演示文档有错误，不提供下载了。待新的演示文档出来。）

许多朋友说上面的DEMO用不了。fusioncharts官方的演示非常不错，就是来不及整理，各位大侠们可以研究一下。网址：[http://www.fusioncharts.com/Demos/ExportChart/Contents/batch\_export.html][http_www.fusioncharts.com_Demos_ExportChart_Contents_batch_export.html]

    | ----------------------- | -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | 属性名                     | 类型                   | 描述                                                                                                                                                                                      |
    | [FusionCharts][] V3核心导出功能相关的属性                                                                                                                                                                                                         |||
    | exportEnabled           | Boolean (0/1)        | 图表是否允许导出images/PDFs?                                                                                                                                                                    |
    | exportShowMenuItem      | Boolean (0/1)        | 是否将导出图片等按钮出现在图表右键菜单中                                                                                                                                                                    |
    | exportFormats           | String               | 格式的列表图表将显示在上下文菜单中，同时为每一个标签。  该属性的值应该分开键值对。分隔符字符将要采用的'|'（分字符）。该属性值的语法如下： KEY=Value\[|KEY=Value\]\*  例如：自定义上下文菜单PNG和PDF格式。  **exportFormats:”PNG=导出png格式图片|JPG=导出jpg格式图片|PDF=导出pdf格式文件”** |
    | exportAtClient          | Boolean (0/1)        | 导出到客户端还是服务器端                                                                                                                                                                            |
    | exportHandler           | String               | 在服务器导出方面而言，这指的是服务器端输出处理程序（已经可以使用的脚本，我们提供的路径）。  在客户导出方面而言，这指的是DOM的组成部分的FusionCharts的导出是在你的网页嵌入，随着图表同上。                                                                                   |
    | exportAction            | 'save' or 'download' | 在服务器端的情况下导出，行动指定是否导出的图像将被发送回客户端的下载，或者是否会在服务器上保存。                                                                                                                                        |
    | exportTargetWindow      | \_self or \_blank    | 在服务器端的情况下使用时，导出作为行动的下载，这个左派配置是否返回图片/ PDF格式将在同一窗口中打开作为附件下载（），或是否会打开一个新窗口。                                                                                                                |
    | exportCallback          | String               | 名称的JavaScript函数将被调用时返回进程的情况下导出成品：  客户端的导出 批量导出 服务器端导出使用'保存'的行动                                                                                                                          |
    | exportFileName          | String               | 利用输出（导出）您可以指定此属性的名称（不包括扩展名）文件。                                                                                                                                                          |
    |                         |                      |                                                                                                                                                                                         |
    | 导出对话框配置相关的属性：                                                                                                                                                                                                                          |||
    | showExportDialog        | Boolean (0/1)        | 是否要显示在捕获阶段的出口对话框。如果没有，开始捕获过程，但没有图表对话框可见。                                                                                                                                                |
    | exportDialogMessage     | String               | 该消息被显示在对话框中。默认为“捕捉数据：”                                                                                                                                                                  |
    | exportDialogColor       | Hex Color            | 对话框背景颜色。                                                                                                                                                                                |
    | exportDialogBorderColor | Hex Color            | 对话框前景颜色。                                                                                                                                                                                |
    | exportDialogFontColor   | Hex Color            | 对话框文本的字体颜色。                                                                                                                                                                             |
    | exportDialogPBColor     | Hex Color            | 对话框进度条的颜色。                                                                                                                                                                              |

\---------------------------------------------------------------------------------------

以上博文分别转至：

http://blog.csdn.net/niuxl521/article/details/7731751


http://www.hkeswl.com/forum.php?mod=viewthread&tid=20019


http://www.cnblogs.com/ATree/archive/2010/07/21/FusionCharts-Export-Image-Pdf.html


随后有时间的话,我会将个人的东西发表该博文中,上述内容有部分删减。


[Debug Mode]: http://docs.fusioncharts.com/flex/charts/Contents/debug_window.html
[FCErrorEvent]: http://docs.fusioncharts.com/flex/charts/Contents/debug_flex.html
[Setting Flash Security Policies]: http://docs.fusioncharts.com/flex/charts/Contents/installation_securitySetting.html
[QNV7-FII3-YAZE.jpg]: /pro/os/crawler/QNV7-FII3-YAZE.jpg
[http_www.fusioncharts.com_Demos_ExportChart_Contents_batch_export.html]: http://www.fusioncharts.com/Demos/ExportChart/Contents/batch_export.html
[FusionCharts]: http://www.cnblogs.com/ATree/archive/2010/04/20/FusionCharts3v-flash.html