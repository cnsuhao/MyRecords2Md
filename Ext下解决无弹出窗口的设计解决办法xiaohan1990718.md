---
title: Ext下解决无弹出窗口的设计解决办法
date: 2014-02-18 18:06:21
categories: "开发"
tags:
	- Ext

---

该思路起源于在页面设计的时候,个人对弹出窗口这类的交互设计非常厌恶.而ext是单页面应用.我也不想通过切换tab页的方式来进行整个功能页面操作.因此衍生了在同一个页面里进行功能的切换.

比如:一个功能页面里包含”新增”、”修改”、”查询列表”的功能. 当我触发”新增”时,大多我看到的设计是弹出一个新增窗口或者新添加一个tab页面等等方式.但个人还是不喜欢,因此想在这个功能页面里做些手脚。

思路如下:

1、 一般来讲这类的切换页面最直接暴力的方式是通过组件”移除”和”添加” 的方式来对同一个页面进行切换的。但这种频繁的remove、add的方式(并且是假移除这种方式)相对之后的解决方案还是消耗性能的。

2、 后来发现另一种’图巧’的方式,即通过布局的方式来解决这种情况.

因为ExtJS提供了一种”card”卡片式布局.这样便解决了无弹窗的方式.

3、 后来在做的过程中,发现如果同时加载很多页面的话,确实量大,为了加快其渲染.借鉴了多tab页面里延迟加载的想法.

Ext.define('Ext.ux.panel.CardPage',\{

extend:'Ext.panel.Panel',

tabItems:\[\],

border:false,

getCurrentPanel:function()\{//获取当前页对象

returnthis.tabItemMap\[this.getLayout().getActiveItem().name\].inited;

\},

getComponetByName:function(name)\{

vartabItemMap = this.tabItemMap;

if(tabItemMap\[name\])\{

if(tabItemMap\[name\].inited)\{

returntabItemMap\[name\].inited;

\}else\{

throw'名称为:'+name+'的tab页面shang未初始化！'

\}

\}else\{

throw'名称为:'+name+'的tab页面不存在！';

\}

\},//转向对应的面板

swicthToByName:function(name,callBack,scope)\{

var tabItemMap = this.tabItemMap;

if(tabItemMap\[name\])\{

if(tabItemMap\[name\].inited)\{

\}else\{

varcom = tabItemMap\[name\].createCom();

if(callBack)callBack.call(scope||this,com,tabItemMap\[name\].inited);

tabItemMap\[name\].inited= com;

this.getLayout().getActiveItem().add(com);

\}

this.getLayout().setActiveItem(tabItemMap\[name\].inited);

\}else\{

throw'名称为:'+name+'的card页面不存在！';

\}

\},

initComponent:function()\{

varme = this,tabItems = this.tabItems,items = \[\],tabItemMap = this.tabItemMap =\{\},obj = null,num = 0,name = null;

for(vari=0,len = tabItems.length;i<len;i++)\{

obj= \{

xtype:’container’,

name: tabItems\[i\]\['name'\],

layout:'fit',

border:false

\};

if(tabItems\[i\]\['init'\]==true)\{

num= i;

name= tabItems\[i\]\['name'\];

\}

tabItemMap\[tabItems\[i\]\['name'\]\]= tabItems\[i\];

items.push(obj);

\}

if(name==null)\{//此时num为默认的 0

name= tabItems\[0\]\['name'\];

\}

this.layout= 'card';//强制card布局

this.items= items;

this.callParent(arguments);

this.on(\{

scope:this,

beforerender:function(panel)\{//beforerender

this.getLayout().setActiveItem(num);

varcom = tabItemMap\[name\].createCom();

tabItemMap\[name\].inited= com;

this.getLayout().getActiveItem().add(com);

\}

\});

\}

\});

也是没有优化代码。思路与多tab页延迟切换是大致一致的,后话就不提了.有兴趣的自己研究代码,不提供样例了。


\--请尊重原创！如有转载请标注转载地址！
\--改文的资源同步上传到我的资源中,供免费下载
