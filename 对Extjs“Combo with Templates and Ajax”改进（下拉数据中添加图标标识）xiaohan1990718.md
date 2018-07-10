---
title: 对Extjs“Combo with Templates and Ajax”改进（下拉数据中添加图标标识）
date: 2012-12-16 20:22:32
categories: "开发"
tags:
	- Ext
	- js
	- Ajax

---

我们知道Extjs4中提供了一个组件叫做“

# Combo with Templates and Ajax #

” 可以用来动态的查询获取后台的数据，本文讲述的是如何将其稍加改造使其更加符合用户需求。

![ZVEY-JAVF-QUFY.jpg][]这是extjs提供的组件我做了截图。


改进部分：

1、模糊查询和后台交互（暂未做到拼音查询和搜索式的查询）

2、根据数据类型（需要提前将数据分类并将数据的分类类型传到前台）

3、展示依然是下拉框的形式。

\--------------------------------------------------------------------------------------------------------------------------------------------------------------

下面讲思路：

1、先研究了下该组件的实现：

``````````
Ext.require([
    'Ext.data.*',
    'Ext.form.*'
]);

Ext.onReady(function(){

    Ext.define("Post", {
        extend: 'Ext.data.Model',
        proxy: {
            type: 'jsonp',
            url : 'http://www.sencha.com/forum/topics-remote.php',
            reader: {
                type: 'json',
                root: 'topics',
                totalProperty: 'totalCount'
            }
        },

        fields: [
            {name: 'id', mapping: 'post_id'},
            {name: 'title', mapping: 'topic_title'},
            {name: 'topicId', mapping: 'topic_id'},
            {name: 'author', mapping: 'author'},
            {name: 'lastPost', mapping: 'post_time', type: 'date', dateFormat: 'timestamp'},
            {name: 'excerpt', mapping: 'post_text'}
        ]
    });

    ds = Ext.create('Ext.data.Store', {
        pageSize: 10,
        model: 'Post'
    });

    panel = Ext.create('Ext.panel.Panel', {
        renderTo: Ext.getBody(),
        title: 'Search the Ext Forums',
        width: 600,
        bodyPadding: 10,
        layout: 'anchor',

        items: [{
            xtype: 'combo',
            store: ds,
            displayField: 'title',
            typeAhead: false,
            hideLabel: true,
            hideTrigger:true,
            anchor: '100%',

            listConfig: {
                loadingText: 'Searching...',
                emptyText: 'No matching posts found.',

                // Custom rendering template for each item
                getInnerTpl: function() {
                    return '<a class="search-item" href="http://www.sencha.com/forum/showthread.php?t={topicId}&p={id}">' +
                        '<h3><span>{[Ext.Date.format(values.lastPost, "M j, Y")]}<br />by {author}</span>{title}</h3>' +
                        '{excerpt}' +
                    '</a>';
                }
            },
            pageSize: 10
        }, {
            xtype: 'component',
            style: 'margin-top:10px',
            html: 'Live search requires a minimum of 4 characters.'
        }]
    });
});
``````````


研究了其实现代码方知：

ds是其数据源,而

``````````
url : 'http://www.sencha.com/forum/topics-remote.php',则是其向后台发送到连接请求路径。
``````````

fields：

``````````
fields: [
            {name: 'id', mapping: 'post_id'},
            {name: 'title', mapping: 'topic_title'},
            {name: 'topicId', mapping: 'topic_id'},
            {name: 'author', mapping: 'author'},
            {name: 'lastPost', mapping: 'post_time', type: 'date', dateFormat: 'timestamp'},
            {name: 'excerpt', mapping: 'post_text'}
``````````

是后台数据字段映射到前台的字段。

而其他的属性配置如：

``````````
xtype: 'combo',
            store: ds,
            displayField: 'title',
            typeAhead: false,
            hideLabel: true,
            hideTrigger:true,
``````````

可以根据需要进行相应的配置（属性配置跟使用一般的combo一样，这个例子中将下拉的图形样式的属性去掉了，如果要实现跟下拉框同样的效果显示,在api里找相应的属性设置即可）

若实现下拉的样式控制，则要看listconfig了。

``````````
listConfig
``````````

根据api显示其可设置的仅有10个属性配置项

``````````
listConfig : Object
An optional set of configuration properties that will be passed to the Ext.view.BoundList's constructor. Any configuration that is valid for BoundList can be included. Some of the more useful ones are:

Ext.view.BoundList.cls - defaults to empty
Ext.view.BoundList.emptyText - defaults to empty string
Ext.view.BoundList.itemSelector - defaults to the value defined in BoundList
Ext.view.BoundList.loadingText - defaults to 'Loading...'
Ext.view.BoundList.minWidth - defaults to 70
Ext.view.BoundList.maxWidth - defaults to undefined
Ext.view.BoundList.maxHeight - defaults to 300
Ext.view.BoundList.resizable - defaults to false
Ext.view.BoundList.shadow - defaults to 'sides'
Ext.view.BoundList.width - defaults to undefined (automatically set to the width of the ComboBox field if matchFieldWidth is true)
``````````


另外在样例中的getInnerTpl方法却没有,怎么办呢？ 不急细细看代码可以看到其是 e  Ext.view.BoundList 's里的。因为本文强调改造这个下拉框,所以就不往后说boundlist了。

动手改改getInnerTpl里的html就可以了。其中\{\}里的元素就是对应feilds的mapping代码。因而可以想到如果要更改下拉里的样式,那么就要从这里入手了～～

思路其实很简单,前面说了，要实现的是分类的图标标识，所以后台必然要将分类的类型传递给前台,怎么传递呢?

前面也说了url属性后的url路径即是请求路径,那么具体的请求就更改url就行了。

那如何实现模糊查询呢？ 那就自己在后台写啦·～～最简单的sql语句里使用like '%parmas%'就行了。

另外如何设置触发这条url呢?

阅读API不难发现：

minChars  : Number

The minimum number of characters the user must type before autocomplete and typeAhead activate.

Defaults to `4` if `queryMode = 'remote'` or `0` if `queryMode = 'local'`, does not apply if `editable = false`.


最后就是美工的活了。前台既然给了类型,那就容易定义css class了，根据不同的类型定义相应的样式（当然太多的话，也不轻松）,举个简单的例子：

将：

``````````
return '<a class="search-item" href="http://www.sencha.com/forum/showthread.php?t={topicId}&p={id}">' +
                        '<h3><span>{[Ext.Date.format(values.lastPost, "M j, Y")]}<br />by {author}</span>{title}</h3>' +
                        '{excerpt}' +
                    '</a>';
``````````

简单更改为 return '<div class="\{style\}">\{mc\}</div>' 其中style是fileds里根据后台的类型参数对应的mapping字段里的内容。 mc也是 这里分别指：类型、名称。

样式里设置相应的背景图片即可。

效果：

![V36F-VJQU-AYYI.jpg][]


仔细阅读api的话，有更好的方法改造它，以上仅仅是简单的更改,设计到复杂些的更改估计就得往boundlist里专研了～

仅供了解学习,因为水平太差，讲不好道不明，本文仅为了证明可以这么做，或许有更好的方法提供出来供网友学习。（应网友请求,将其发到csdn，欢迎指正）








[ZVEY-JAVF-QUFY.jpg]: /pro/os/crawler/ZVEY-JAVF-QUFY.jpg
[V36F-VJQU-AYYI.jpg]: /pro/os/crawler/V36F-VJQU-AYYI.jpg