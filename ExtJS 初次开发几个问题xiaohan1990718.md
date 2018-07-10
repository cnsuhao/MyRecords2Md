---
title: ExtJS 初次开发几个问题
date: 2012-02-02 11:01:38
categories: "开发"
tags:
	- Ext
	- JS

---

之前做过一阵的ExtJs开发，从最傻的符号问题，到后来的渲染问题都碰到过。下面是个人开发过程中做的一些总结，多是问题的应对：

1、引入js和css文件时注意文件的路径问题；

2、导入ext-base.js后注意设置Ext.BLANK\_IMAGE\_URL的值（透明图片s.gif位置）；

3、IE提示“缺少标识符，字符串或数字”错误，为配置时 “\}”前多了逗号，且所处位置在Ext.onReady在同一个js文件

4、IE提示变量未定义，一般为有语法错误，如多余了”,”等

5、“无效字符”错误，可能是将”,”打成了”，”了

6、”缺少’\}'”错误，问题是多个参数之间缺少了”,”导致

7、加载文件较多时，使用ExtJs可做一个加载提示

8、ext-base.js引入必须在ext-all.js之前

9、Ext.state.Manager.setProvider(new Ext.state.CookieProvider());
初始化Ext状态管理器，在Cookie中记录用户的操作状态，如果不启用，象刷新时就不会保存当前的状态，而是重新加载
如果窗口中有用可拖动面板的话，你在拖动后如果启动了Ext.state.Manager.setProvider(new Ext.state.CookieProvider()),就算刷新后面板仍然会在你拖动后的位置。如果不启用的话是不是就会按照默认的排列方式排列

10、对浏览器禁用javascript的提示
<noscript class=”noticeDialog”><div>
<p>请开启浏览器的 JavaScript 支持！否则不能正常使用本系统<br>启用之后，刷新页面即可</p>
</div></noscript>

11、TabPanel中放置复杂组件时，注意需要设置TabPanel的Width

12、extjs中文字体在firefox里显示偏小的问题，解决方法是再创建一个名为ext-patch.css的css文件，内容见http://www.phpchina.com/html/78/78-28624.html

13、中文化问题，在 ext-all.js 后面，挂上 ext-lang-zh\_CN.js 即可，如：
<script type=”text/javascript” src=”<%=contextPath%>/public/js/ext-base.js”></script>
<script type=”text/javascript” src=”<%=contextPath%>/public/js/ext-all.js”></script>
<script type=”text/javascript” src=”<%=contextPath%>/public/js/ext-lang-zh\_CN.js”></script>

14、ComboBox加载后自动选择第一项
var pn\_zlfx\_cbb\_grade = new Ext.form.ComboBox(\{
displayField: ‘name’,
valueField: ‘id’,
triggerAction: ‘all’,
width: 80,
lazyInit: false,
mode: ‘local’,
editable: false,
forceSelection: true,
store: new Ext.data.JsonStore(\{
url: SITE\_URL+’get\_zlfx.asp?grade\_id=0′,
fields: \['id', 'name'\],
root: ‘data’,
autoLoad: true,
listeners:\{
load: function(store, records, options)\{
if (records.length != 0)\{
pn\_zlfx\_cbb\_grade.setValue(records\[0\].data.id);
\}
\}
\}
\})

15、JsonStore排序：sortInfo: \{field: “name”, direction: “ASC|DESC”\}

16、Extjs类的配置属性是不能被动态配置的,就如同上面这样的写法,当然,可能可以通过调用或设置某些公共的方法和属性来改变这些配置属性,但不能直接设置.

17、xhtml strict模式：<!DOCTYPE html
PUBLIC “-//W3C//DTD XHTML 1.0 Strict//EN”
“http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd“>

18、Html文档<script>标签中的js代码放在<!\[CDATA\[和\]\]>之间，使xhtml验证时忽略中间的内容

19、JsonStore添加Record
var orgaListRecord = new Ext.data.Record.create(\[
\{name: 'id\_combo1'\},
\{name : 'name'\}
\]);
var orgaList = new Ext.data.JsonStore(\{
url: ‘./get\_organisme.php’,
root: ‘orga’,
fields: orgaListRecord
\});
orgaList.add(\[
new orgaListRecord(\{'id\_combo1': 3,'name': 'Other'\})
\]);
orgaList.load(\{add: true\}); // instead of orgaList.load();

20、数据加载前须判断是否有数据：
if (typeof(pn\_zlfx\_gp\_mark\_subjects)==”undefined” || typeof(pn\_zlfx\_gp\_mark\_subjects.data)==”undefined”)
pn\_zlfx\_gp\_setting.store.removeAll();
else
pn\_zlfx\_gp\_setting.store.loadData(pn\_zlfx\_gp\_mark\_subjects);

21、尽量将处理过程放在关键位置，减少重复代码

22、默认排序方式：sortDir

23、服务器返回的JSON数据字段用’或”括起来，避免与js关键词与保留字冲突

24、Ext.encode()出错，问题是json数据有问题

25、store增加totalProperty配置，可用store.getCount()获取

26、store中文排序异常问题，通过重载Ext.data.Store.prototype.applySort函数来解决

27、store.load()是异步操作，如果希望在数据载入后再执行操作，可以通过以下方式执行：
store.load(\{
callback: function (records) \{
alert(store.getTotalCount());
\}
\});

28、 EXTJS items不能显示问题收藏
有时经常碰到添加items时不能显示，郁闷不得行~~
经不断测试可能有如下几个方法可以解决：
1)试添加：Layout:’fit’
2)若xtype为’panel’，可试添加：
listeners: \{
‘activate’: function(p) \{p.doLayout();\}
,single:true
\}
3)若xtype为’tabpanel’，可试添加：
layoutOnTabChange:true
总的来说是cmp.doLayout()问题….

29、Ext.ajax.request增加form和isUpload:true时，请求方式为enctype=”multipart/form-data”传值，asp中需特别解析

30、ComboBox增加mode:’local’配置来手动控制数据载入，同时可防止数据的二次加载

31、要使ComboBox手动输入的值能得到提交，需要增加ComboBox的Blur事件处理函数
onComboBoxBlur: function(field)\{
field.setValue(field.getRawValue());
\}

32、Firebug显示所有Extjs组件事件
Ext.util.Observable.prototype.fireEvent = Ext.util.Observable.prototype.fireEvent.createInterceptor(function() \{
console.log(arguments);
return true;
\});

33、工具栏Toolbar内容的增删
var tb = this.gp.getTopToolbar();
// 删除工具栏内容
var i = tb.items.getCount();
while(i)\{
Ext.fly(tb.items.get(i).getEl()).remove();
tb.items.removeAt(i);
\}
// 重新添加工具栏内容
tb.add(‘包括（’, this.cbgType, ‘）’);

34、Ext.ux.form.LovCombo不能设置forceSelection为true，否则当控件失去焦点时显示值会清空

35、IE下Ext.GridPanel的autoWidth或者layout:’fit’会将宽度拉的很长的解决办法：

var grid = new Ext.grid.GridPanel(\{

bodyStyle:’width:100%’,

autoWidth:true,

…….

\});
36、Ext.GridPanel加了CellSelection后，如设置viewConfig:\{forceFit:true\}点击单元格会导致表格跳动
解决方案：[http://www.extjs.com/forum/showthread.php?p=282928\#post282928][http_www.extjs.com_forum_showthread.php_p_282928_post282928]

37、ComboBox执行Filter第一次无效的解决是 增加lastQuery:” 配置

38、Panel内部高度自适应+自动出现滚动条，增加以下配置
,autoScroll: true
,style:”height:100%;”
,bodyStyle:”height:100%;”

39、某些情况下TabPanel里加载iframe第一次不显示，可在panel显示后重新设置一下iframe的src强制加载一遍


最后学习Extjs的地方：

学习extjs4.0的教程


http://www.uspcat.com/forum.php?mod=viewthread&tid=197&extra=


--------------------

[枫芸志][Link 1]原创文章，转载请注明来源并保留原文链接

本文链接:http://witmax.cn/extjs-notes.html


[http_www.extjs.com_forum_showthread.php_p_282928_post282928]: http://www.extjs.com/forum/showthread.php?p=282928#post282928
[Link 1]: http://witmax.cn/