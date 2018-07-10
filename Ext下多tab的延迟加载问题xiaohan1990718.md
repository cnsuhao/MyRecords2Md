---
title: Ext下多tab的延迟加载问题
date: 2014-02-18 18:04:43
categories: "开发"
tags:
	- Ext

---

声明 : 名称或许跟我实际要说的不一致.我只是这么理解的而已.

有时候在ext开发框架下,我们会用到多tab的情况.但很多时候我发现(包括我之前)都是这么写的:

Var tabPanel = Ext.create(‘Ext.tab.Panel’,\{

Items:\[\{

title: ’tab1’

\},\{

title : ’tab2’

\}//…. 等等很多的页面

\]

\});

假如每个tab里包含大量的请求的话,那么这么做在第一次加载时恐怕会变的相当的耗费时间.（我假设场景是多请求以及数据量较大的情况）,当然我们可以通过设计来屏蔽这种场景的出现.但并非我讨论范围内.

那么我们如何解决这类问题呢? //注：这种办法原解决人为我前一公司同事,当时我俩讨论的结果.不过这里我重新做了一套,思路同之前.并且没有优化.

Ext.define(‘Ext.ux.tab.switchTabPanel',\{

extend:'Ext.tab.Panel',

tabItems:\[\],

border:false,

getCurrentPanel:function()\{

returnthis.tabItemMap\[this.activeTab.name\].inited;

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

\},

initComponent:function()\{

varme = this,tabItems = this.tabItems,items = \[\],tabItemMap = this.tabItemMap =\{\},obj = null,num = 0,name = null;

for(vari=0,len = tabItems.length;i<len;i++)\{

obj= \{

name:tabItems\[i\]\['name'\],

title:tabItems\[i\]\['title'\],

layout:'fit',

xtype:’container’,

border:false

\};

if(tabItems\[i\]\['init'\]==true)\{

num= i;

name= tabItems\[i\]\['name'\];

\}

tabItemMap\[tabItems\[i\]\['name'\]\]= tabItems\[i\];

items.push(obj);

\}

if(name== null)\{

name= tabItems\[0\]\['name'\];//如果未指定初始的面板,则默认第一个

\}

this.items = items;

this.activeTab= num;

this.callParent(arguments);

this.on(\{

beforerender:function(tabPanel)\{

varnewCard = tabPanel.getComponent(num);

varpanel = tabItemMap\[name\];

varcom = panel.createCom();

tabItemMap\[name\].inited= com;//标记该页面已经初始化

deletepanel.title;

newCard.add(com);

\},

beforetabchange:function(tabPanel,newCard,oldCard,eOpts)\{

if(!tabItemMap\[newCard\['name'\]\].inited)\{//如果没初始化过

varpanel = tabItemMap\[newCard.name\];

var com = panel.createCom();

tabItemMap\[newCard.name\].inited= com;

deletepanel.title;

newCard.add(com);

\}

\}

\});

\}

\});

上述代码基本上还没做优化,暂时就这样.它解决的思路是:

根据tabItems的配置. 在原生tabPanel组件基础之上,生成了多个“假”的tab页面(此时尚未初始化每个tab里的组件或其他对象).在切换tab页前才对改tab页面里的真实数据或组件这些东西进行初始加载。

其使用方式(保证name唯一性,当然属性名可自己定制):

使用样例:

var panel = Ext.create(' Ext.ux.tab.switchTabPanel ',\{

tabItems:\[\{

name:'1',

title:'XXXX',

createCom:function()\{

returnExt.create('Ext.panel.Panel',\{

html:'XXX'

\});

\}

\},\{

name:'2',

title:'aaa',

createCom:function()\{

returnExt.create('Ext.panel.Panel',\{

html:'AAAA'

\});

\}

\},\{

name:'3',

title:'CCCCC',

init:true,

createCom:function()\{

returnExt.create('Ext.panel.Panel',\{

html:'CCCC'

\});

\}

\}\]

\});

\--请尊重原创！如有转载请标注转载地址！
\--改文的资源同步上传到我的资源中,供免费下载

