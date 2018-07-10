---
title: Extjs-Combobox三级联动
date: 2011-12-22 19:21:33
categories: "开发"
tags:
	- Ext
	- js

---



很多网友在问，Extjs4.0 ComboBox如何实现，好在之前用3.x实现过一个三级联动，如今用Extjs4.0来实现同样的联动效果。其中注意的一点就是，3.x中的model:'local'在Extjs4.0中用queryMode: 'local'来表示，而且在3.x中Load数据时用reload，但是在extjs4.0中要使用load来获取数据。如下图：

![EIAQ-JJ6Z-BFUJ.gif][]


**代���部分**

先看HTML代码：

1.  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN""http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
2.  <html xmlns="http://www.w3.org/1999/xhtml">
3.  <head> 
4.  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
5.  <title>MHZG.NET-城市三级联动实例</title> 
6.  <link rel="stylesheet" type="text/css" href="../../resources/css/ext-all.css" />
7.  <script type="text/javascript" src="../../bootstrap.js"></script>
8.  <script type="text/javascript" src="../../locale/ext-lang-zh\_CN.js"></script>
9.  <script type="text/javascript" src="combobox.js"></script>
10. </head> 
11. 
    
12. <body> 
13. </body> 
14. </html> 

``````````
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>MHZG.NET-城市三级联动������</title>
<link rel="stylesheet" type="text/css" href="../../resources/css/ext-all.css" />
<script type="text/javascript" src="../../bootstrap.js"></script>
<script type="text/javascript" src="../../locale/ext-lang-zh_CN.js"></script>
<script type="text/javascript" src="combobox.js"></script>
</head>

<body>
</body>
</html>
``````````

简单的很，就是加载了基本的CSS文件和JS文件，并且加载自定义的combobox.js文件。

combobox.js：

1.  Ext.require('Ext.\*');
2.  Ext.onReady(function()\{ 
3.  //定义ComboBox模型 
4.   Ext.define('State', \{ 
5.   extend: 'Ext.data.Model',
6.   fields: \[ 
7.   \{type: 'int', name: 'id'\},
8.   \{type: 'string', name: 'cname'\}
9.   \] 
10.  \}); 
11. 
12. //加载省数据源 
13. var store = Ext.create('Ext.data.Store', \{
14.  model: 'State', 
15.  proxy: \{ 
16.  type: 'ajax', 
17.  url: 'city.asp?act=sheng&n='\+new Date().getTime()+''
18.  \}, 
19.  autoLoad: true,
20.  remoteSort:true
21.  \}); 
22. //加载��数据�� 
23. var store1 = Ext.create('Ext.data.Store', \{
24.  model: 'State', 
25.  proxy: \{ 
26.  type: 'ajax', 
27.  url: 'city.asp?act=shi&n='\+new Date().getTime()+''
28.  \}, 
29.  autoLoad: false,
30.  remoteSort:true
31.  \}); 
32. //加载区数据源 
33. var store2 = Ext.create('Ext.data.Store', \{
34.  model: 'State', 
35.  proxy: \{ 
36.  type: 'ajax', 
37.  url: 'city.asp?act=qu&n='\+new Date().getTime()+''
38.  \}, 
39.  autoLoad: false,
40.  remoteSort:true
41.  \}); 
42. 
43. 
44. 
45.  Ext.create("Ext.panel.Panel",\{
46.  renderTo: document.body, 
47.  width:290, 
48.  height:220, 
49.  title:"城市三级联动",
50.  plain: true, 
51.  margin:'30 10 0 80',
52.  bodyStyle: "padding: 45px 15px 15px 15px;",
53.  defaults :\{ 
54.  autoScroll: true, 
55.  bodyPadding: 10 
56.  \}, 
57.  items:\[\{ 
58.  xtype:"combo", 
59.  name:'sheng',
60.  id : 'sheng', 
61.  fieldLabel:'选择省',
62.  displayField:'cname', 
63.  valueField:'id',
64.  store:store, 
65.  triggerAction:'all',
66.  queryMode: 'local', 
67.  selectOnFocus:true,
68.  forceSelection: true,
69.  allowBlank:false,
70.  editable: true, 
71.  emptyText:'请选择省',
72.  blankText : '请选择省', 
73.  listeners:\{ 
74.  select:function(combo, record,index)\{
75. try\{
76. //userAdd = record.data.name;
77. var parent=Ext.getCmp('shi');
78. var parent1 = Ext.getCmp("qu");
79.  parent.clearValue(); 
80.  parent1.clearValue(); 
81.  parent.store.load(\{params:\{param:this.value\}\});
82.  \} 
83. catch(ex)\{
84.  Ext.MessageBox.alert("错误","数据加载失败。");
85.  \} 
86.  \} 
87.  \} 
88.  \}, 
89.  \{ 
90.  xtype:"combo", 
91.  name:'shi',
92.  id : 'shi', 
93.  fieldLabel:'选择市',
94.  displayField:'cname', 
95.  valueField:'id',
96.  store:store1, 
97.  triggerAction:'all',
98.  queryMode: 'local', 
99.  selectOnFocus:true,
100.  forceSelection: true,
101.  allowBlank:false,
102.  editable: true, 
103.  emptyText:'请选择市',
104.  blankText : '请选择市', 
105.  listeners:\{ 
106.  select:function(combo, record,index)\{
107. try\{
108. //userAdd = record.data.name;
109. var parent = Ext.getCmp("qu");
110.  parent.clearValue(); 
111.  parent.store.load(\{params:\{param:this.value\}\});
112.  \} 
113. catch(ex)\{
114.  Ext.MessageBox.alert("错误","数据加载失败。");
115.  \} 
116.  \} 
117.  \} 
118.  \}, 
119.  \{ 
120.  xtype:"combo", 
121.  name:'qu',
122.  id : 'qu', 
123.  fieldLabel:'选择区',
124.  displayField:'cname', 
125.  valueField:'id',
126.  store:store2, 
127.  triggerAction:'all',
128.  queryMode: 'local', 
129.  selectOnFocus:true,
130.  forceSelection: true,
131.  allowBlank:false,
132.  editable: true, 
133.  emptyText:'请选择区',
134.  blankText : '请选择区', 
135.  \} 
136.  \] 
137.  \}) 
138. \});

``````````
Ext.require('Ext.*');
Ext.onReady(function(){
  //定义ComboBox模型
  Ext.define('State', {
	  extend: 'Ext.data.Model',
	  fields: [
		  {type: 'int', name: 'id'},
		  {type: 'string', name: 'cname'}
	  ]
  });
 
 //加载省数据源
  var store = Ext.create('Ext.data.Store', {
	  model: 'State',
	  proxy: {
		  type: 'ajax',
		  url: 'city.asp?act=sheng&n='+new Date().getTime()+''
	  },
	  autoLoad: true,
	  remoteSort:true
  });
  //加载市数据源
  var store1 = Ext.create('Ext.data.Store', {
	  model: 'State',
	  proxy: {
		  type: 'ajax',
		  url: 'city.asp?act=shi&n='+new Date().getTime()+''
	  },
	  autoLoad: false,
	  remoteSort:true
  });
  //加载区数据源
  var store2 = Ext.create('Ext.data.Store', {
	  model: 'State',
	  proxy: {
		  type: 'ajax',
		  url: 'city.asp?act=qu&n='+new Date().getTime()+''
	  },
	  autoLoad: false,
	  remoteSort:true
  });
	
  
  
  Ext.create("Ext.panel.Panel",{
	renderTo: document.body,
	width:290,
	height:220,
	title:"城市三级联动",
	plain: true,
	margin:'30 10 0 80',
	bodyStyle: "padding: 45px 15px 15px 15px;",
	defaults :{
		autoScroll: true,
		bodyPadding: 10
	},
	items:[{
		xtype:"combo",
		name:'sheng',
		id : 'sheng',
		fieldLabel:'选择省',
		displayField:'cname',
		valueField:'id',
		store:store,
		triggerAction:'all',
		queryMode: 'local', 
		selectOnFocus:true,
		forceSelection: true,
		allowBlank:false,
		editable: true,
		emptyText:'请选择省',
		blankText : '请选择省',
		listeners:{   
			select:function(combo, record,index){
				 try{
					 //userAdd = record.data.name;
					 var parent=Ext.getCmp('shi');
					 var parent1 = Ext.getCmp("qu");
					 parent.clearValue();
					 parent1.clearValue();
					 parent.store.load({params:{param:this.value}});
				 }
				 catch(ex){
					 Ext.MessageBox.alert("错误","数据加载失败。");
				 }
			}
		}
		},
		{
		xtype:"combo",
		name:'shi',
		id : 'shi',
		fieldLabel:'选择市',
		displayField:'cname',
		valueField:'id',
		store:store1,
		triggerAction:'all',
		queryMode: 'local',
		selectOnFocus:true,
		forceSelection: true,
		allowBlank:false,
		editable: true,
		emptyText:'请选择市',
		blankText : '请选择市',
		listeners:{   
			select:function(combo, record,index){
				 try{
					 //userAdd = record.data.name;
					 var parent = Ext.getCmp("qu");
					 parent.clearValue();
					 parent.store.load({params:{param:this.value}});
				 }
				 catch(ex){
					 Ext.MessageBox.alert("错误","数据加载失败。");
				 }
			}
		}
		},
		{
		xtype:"combo",
		name:'qu',
		id : 'qu',
		fieldLabel:'选择区',
		displayField:'cname',
		valueField:'id',
		store:store2,
		triggerAction:'all',
		queryMode: 'local',
		selectOnFocus:true,
		forceSelection: true,
		allowBlank:false,
		editable: true,
		emptyText:'请选择区',
		blankText : '请选择区',
		}
	]
  })
});
``````````

上述代码中，如果在ComboBox直接定义store数据源，会出现这样一种情况，那就是当选择完第一个省，点击第二个市的时候，会闪一下，再点击，才会出现市的数据。那么要解决这样的情况，那么必须先要定义好省、市、区的数据源。那么再点击的时候，就不会出现上述情况了。

代码中，使用store为省的数据，设置其autoLoad为true，那么页面第一次加载的时候，就会自动加载省的数据，然后给省和市添加了监听select，作用在于当点击省的时候，要清空市和区的数据，并根据当前选定的值去加载对应的数据到市的数据源中。当然store1和store2原理是一样的。

最后，服务端要根据传的值进行数据检索及返回正确数据，这里没有从��据库中��询数据，而只是简单的写了一些测试代码，相信extjs代码没有任何的问题了，那么服务端返回数据，就不是一件很重要的事情了。

City.asp：

1.  <%@LANGUAGE="VBSCRIPT" CODEPAGE="65001"%>
2.  <% 
3.   Response.ContentType = "text/html"
4.   Response.Charset = "UTF-8"
5.  %> 
6.  <% 
7.  Dim act:act = Request("act")
8.  Dim param : param = Request("param")
9.  If act = "sheng"Then
10.  Response.Write("\[") 
11.  Response.Write("\{""cname"":""北京市"",""id"":""110000""\},")
12.  Response.Write("\{""cname"":""内蒙古自治区"",""id"":""150000""\}")
13.  Response.Write("\]")
14. EndIf
15. If act = "shi"Then
16. If param = "110000"Then
17.  Response.Write("\[")
18.  Response.Write("\{""cname"":""市辖区"",""id"":""110100""\},")
19.  Response.Write("\{""cname"":""市辖县"",""id"":""110200""\}")
20.  Response.Write("\]")
21. ElseIf param = "150000"Then
22.  Response.Write("\[")
23.  Response.Write("\{""cname"":""呼和浩特市"",""id"":""150100""\},")
24.  Response.Write("\{""cname"":""包头市"",""id"":""150200""\}")
25.  Response.Write("\]")
26. EndIf
27. EndIf
28. If act = "qu"Then
29. If param = "110100"Then
30.  Response.Write("\[")
31.  Response.Write("\{""cname"":""朝阳区"",""id"":""110101""\},")
32.  Response.Write("\{""cname"":""昌平区"",""id"":""110102""\}")
33.  Response.Write("\]")
34. ElseIf param = "110200"Then
35.  Response.Write("\[")
36.  Response.Write("\{""cname"":""密云县"",""id"":""110201""\},")
37.  Response.Write("\{""cname"":""房山县"",""id"":""110202""\}")
38.  Response.Write("\]")
39. ElseIf param = "150100"Then
40.  Response.Write("\[")
41.  Response.Write("\{""cname"":""回民区"",""id"":""150101""\},")
42.  Response.Write("\{""cname"":""新城区"",""id"":""150102""\}")
43.  Response.Write("\]")
44. ElseIf param = "150200"Then
45.  Response.Write("\[")
46.  Response.Write("\{""cname"":""青山区"",""id"":""150201""\},")
47.  Response.Write("\{""cname"":""东河区"",""id"":""150202""\}")
48.  Response.Write("\]")
49. EndIf
50. EndIf
51. %> 

``````````
<%@LANGUAGE="VBSCRIPT" CODEPAGE="65001"%>
<%
	Response.ContentType = "text/html"
	Response.Charset = "UTF-8"
%>
<%
	Dim act:act = Request("act")
	Dim param : param = Request("param")
	If act = "sheng" Then
		Response.Write("[")
		Response.Write("{""cname"":""北京市"",""id"":""110000""},")
		Response.Write("{""cname"":""内蒙古自治区"",""id"":""150000""}")
		Response.Write("]")
	End If
	If act = "shi" Then
		If param = "110000" Then
			Response.Write("[")
			Response.Write("{""cname"":""市辖区"",""id"":""110100""},")
			Response.Write("{""cname"":""市辖县"",""id"":""110200""}")
			Response.Write("]")
		ElseIf param = "150000" Then
			Response.Write("[")
			Response.Write("{""cname"":""呼和浩特市"",""id"":""150100""},")
			Response.Write("{""cname"":""包头市"",""id"":""150200""}")
			Response.Write("]")
		End If
	End If
	If act = "qu" Then
		If param = "110100" Then
			Response.Write("[")
			Response.Write("{""cname"":""朝阳区"",""id"":""110101""},")
			Response.Write("{""cname"":""昌平区"",""id"":""110102""}")
			Response.Write("]")
		ElseIf param = "110200" Then
			Response.Write("[")
			Response.Write("{""cname"":""密云县"",""id"":""110201""},")
			Response.Write("{""cname"":""房山县"",""id"":""110202""}")
			Response.Write("]")
		ElseIf param = "150100" Then
			Response.Write("[")
			Response.Write("{""cname"":""回民区"",""id"":""150101""},")
			Response.Write("{""cname"":""新城区"",""id"":""150102""}")
			Response.Write("]")
		ElseIf param = "150200" Then
			Response.Write("[")
			Response.Write("{""cname"":""青山区"",""id"":""150201""},")
			Response.Write("{""cname"":""东河区"",""id"":""150202""}")
			Response.Write("]")
		End If
	End If
%>
``````````

如果大家有更好的实现方法，请留言指正或加我的QQ群一起分享。QQ群号码：112445964


[EIAQ-JJ6Z-BFUJ.gif]: /pro/os/crawler/EIAQ-JJ6Z-BFUJ.gif