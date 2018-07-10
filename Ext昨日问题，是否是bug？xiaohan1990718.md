---
title: Ext昨日问题，是否是bug？
date: 2012-10-31 23:28:54
categories: "开发"
tags:
	- Ext

---

Ext设计目的是单页面应用，其加载渲染需要在dom节点创建后才能render到该节点，顺序颠倒却不行。

\---这个ext+自定义html元素的想法 搞头大了。

如果ext组件内嵌套div元素，该元素渲染没问题，但是只要内部再嵌套一层ext组件元素就over啊。

``````````
Ext.onReady(function() {

	var container = Ext.create('Ext.container.Container', {
		width : 300,
		height : 400,
        html:'ss',
        items:[{
        	xtype:'button',
        	text:'sub'
        }]
	});
	var div = document.createElement('DIV');
	document.getElementById('myDiv').appendChild(div);//行2
	container.render(div);//行3
	});
``````````


如上述代码，如果iems删除的话 即不添加ext其他组件的话，行2和行3的顺序是可以换的（div添加到指定位置-id为myDiv之前 container组件是可以渲染出数据的）。但是如果包含ext组件的话，换顺序的话就会报昨天博文中的错误。如上述顺序的话是自己创建的div先放到myDiv处，即添加到dom树上之后才能调用render方法。

Extjs官网能给个回复不？迷茫了都。