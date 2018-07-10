---
title: 自己写ExtJS 4控件： Combobox内嵌入Tree
date: 2012-06-19 17:22:10
categories: "开发"
tags:
	- Ext
	- JS

---

Combobox内嵌入Tree，网上很多代码，但大多属于ExtJS 2或ExtJS 3，我没有找到适用于ExtJS 4的代码，由于ExtJS 4变化蛮大，所以网上的代码都不能直接使用，不过没关系，自己写吧

看了网上的示例代码，其实只要注意几点即可

### 1、为Combobox的tpl插入一个带有id的div ###

``````````
tpl ='<div id="tree"></div>'
``````````

### 2、写Combobox的expand事件 ###

当Combobox展开的时候，要让tree展现到我们之前设置div内

这里要注意一个问题，需要判断，如果tree已经展现过了，则不要重复render

``````````
me.on('expand', function () {
    if (!this.tree.rendered) {
        this.tree.render(this.treeid);
    }
});
``````````

### 3、写tree的itemclick事件，实现tree选择之后给Combobox赋值 ###

首先要设置Combobox的displayField的值

``````````
this.displayField = this.displayField || 'text'
``````````

然后完成tree的itemclick事件，查API知道，itemclick的function如下

``````````
itemdblclick( Ext.view.View this, Ext.data.Model record, HTMLElement item, Number index, Ext.EventObject e, Object eOpts )
``````````

我们只需要用到前面2个参数即可，代码如下

``````````
this.tree.on('itemclick', function (view, record) {
    me.setValue(record);
    me.collapse();//关闭combobox
});
``````````

我已经把嵌套tree的combobox写成了一个component，完整代码如下

``````````
Ext.define('TreeComboBox', {
    extend: 'Ext.form.field.ComboBox',

    url: '',
    tree: {},
    textProperty: 'text',
    valueProperty: '',

    initComponent: function () {
        Ext.apply(this, {
            editable: false,
            queryMode: 'local',
            select: Ext.emptyFn
        });

        this.displayField = this.displayField || 'text',
        this.treeid = Ext.String.format('tree-combobox-{0}', Ext.id());
        this.tpl = Ext.String.format('<div id="{0}"></div>', this.treeid);

        if (this.url) {
            var me = this;
            var store = Ext.create('Ext.data.TreeStore', {
                root: { expanded: true },
  ��             proxy: { type: 'ajax', url: this.url }
            });
            this.tree = Ext.create('Ext.tree.Panel', {
                rootVisible: false,
                autoScroll: true,
                height: 200,
                store: store
            });
            this.tree.on('itemclick', function (view, record) {
                me.setValue(record);
                me.collapse();
            });
            me.on('expand', function () {
                if (!this.tree.rendered) {
                    this.tree.render(this.treeid);
                }
            });
        }
        this.callParent(arguments);
    }
});
``````````

前台代码

``````````
Ext.create('TreeComboBox', {
    renderTo: Ext.getBody(),
    width: 400,
    url: '/models/tree-data.json'
});
``````````

无图无真像，效果截图

[![image][]][image 1]

由于有了这个思路，那我们可以很容易的制作嵌入grid的combobox，而且也可以配合tree的filter，实现combobox的filter效果，这里特别指出，由于ExtJS 4去掉了TreeFitler，要实现fitler还比较麻烦，所以最简单的就是我们自己写一个treefilter，在完全完成这个treecombobox后，我会奉上完整代码



[image]: /pro/os/crawler/NY6B-2YVU-UBI2.jpg
[image 1]: http://images.cnblogs.com/cnblogs_com/letao/201205/201205281641026563.png