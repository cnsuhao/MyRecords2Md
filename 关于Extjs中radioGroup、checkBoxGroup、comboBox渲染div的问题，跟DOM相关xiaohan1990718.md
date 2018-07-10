---
title: 关于Extjs中radioGroup、checkBoxGroup、comboBox渲染div的问题，跟DOM相关
date: 2012-10-30 20:06:51
categories: "开发"
tags:
	- Ext
	- js

---

``````````
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme() + "://"
+ request.getServerName() + ":" + request.getServerPort()
+ path + "/";
%>


<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">


<title>RaidoGroup/CheckBoxGroup/ComboBox</title>


<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
<link rel="stylesheet" type="text/css" href="styles.css">
-->
<link rel="stylesheet" type="text/css"
href="extjs/resources/css/ext-all.css">
<script type="text/javascript" src="extjs/bootstrap.js">
</script>
<script type="text/javascript" src="extjs/ext-all.js">
</script>

</head>
<body>
<div id='checkBoxDiv'></div>
<div id='radioDiv'></div>
<div id='comboBoxDiv'></div>
<script type="text/javascript">
Ext.onReady(function() {
var checkBoxGroup = Ext.create('Ext.form.CheckboxGroup', {
width : 300,
name:'rb',
items : [ {
boxLabel : 'Item 1',
inputValue : '1'
}, {
boxLabel : 'Item 2',
inputValue : '2',
checked : true
}]
});


var radioGroup = Ext.create('Ext.form.RadioGroup',{
width:300,
items: [
            { boxLabel: 'Item 1', name: 'rb', inputValue: '1' },
            { boxLabel: 'Item 2', name: 'rb', inputValue: '2', checked: true}
        ]
});


var states = Ext.create('Ext.data.Store', {
    fields: ['abbr', 'name'],
    data : [
        {"abbr":"AL", "name":"Alabama"},
        {"abbr":"AK", "name":"Alaska"}
    ]
});


var comboBox = Ext.create('Ext.form.ComboBox', {
    fieldLabel: 'Choose State',
    store: states,
    queryMode: 'local',
    displayField: 'name',
    valueField: 'abbr'
});


/**
 * checkBoxGroup 行2放置行3下方即报错：Uncaught TypeError: Cannot read property 'dom' of null 
 */
var checkBoxGroupDiv = document.createElement('DIV');//行1
document.getElementById('checkBoxDiv').appendChild(checkBoxGroupDiv);//行2
checkBoxGroup.render(checkBoxGroupDiv);//行3
//----------------------------raidoGroup
var radioGroupDiv = document.createElement('DIV');
document.getElementById('radioDiv').appendChild(radioGroupDiv);
radioGroup.render(radioGroupDiv);


//----------------------------comboBox
/***
 * 我的疑问：行2 和行3 可以换行 但是checkboxGroup以及raidoGroup为何会报错？
 * 复选、单选组以及下拉的渲染有什么不同么？
 */
var comboBoxDiv = document.createElement('DIV');
document.getElementById('comboBoxDiv').appendChild(comboBoxDiv);//行2
comboBox.render(comboBoxDiv);//行3
});
</script>
</body>
</html>
``````````



以上是源代码。

我们的疑问：

ext 元素怎么将自身的生成的dom 元素render到一个创建的尚未附加到页面dom树上的dom节点上。

如果comboBox行2行3可以换行放置的话，为何checkboxGroup和radioGrou会报错呢？？？？？

这是否是Extjs的一个bug???

附上jsp文件http://download.csdn.net/detail/xiaohan1990718/4703790。将对应的ext需要的js、css引入置换路径即可。