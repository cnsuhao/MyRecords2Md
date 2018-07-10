---
title: EXT2.0 简明教程(七)
date: 2012-02-09 19:23:11
categories: "开发"
tags:
	- EXT

---

## 7.1. 如此拖拽，简直就像与生俱来的本能一样。 ##

你可以拖拽grid里的行，让它们按你的方式去排列。

你可以拖拽tree里的节点，把节点从一个枝干拖向另一个枝干。

grid和tree之间，也可以拖动。

layout的split也是一种拖动，改变布局的大小。

resize也算是拖动，改变大小。

## 7.2. 第一！乱拖。 ##

一直认为拖动很复杂，可实际上只需要一条语句就可以了。

``````````
new Ext.dd.DDProxy('block');
``````````

然后看看html里的部分

``````````
<div id="block" style="background: red;"> </div>
``````````

如果不做任何标记，我们根本什么都看不到，所以我给加上了红色背景颜色。现在你可以乱拖了。

![07-01.png][]

截图里的红条条可以随便拖，实际看例子吧。lingo-sample/1.1.1/07-01.html，2.0里用法一样，lingo-sample/2.0/07-01.html。

## 7.3. 第二！代理proxy和目标target ##

实际上我们刚才看到了DDProxy可以随便拖，另外一个DDTarget是不能拖动的，这东西是用来放DDProxy的一个区域。看一看继承体系图可能有助于我们的理解。

![07-02.png][]

简单来说，左边都是可以随你的鼠标拖动的，拖动起来以后，直接把他们扔到右边那些定义好的区域就好了。proxy是可拖动对象，target是拖动的目的地。

在知道了是与非之后，我们要验证一下自己的理念了。

proxy是我们要拖来拖去的东西。

``````````
var proxy = new Ext.dd.DragSource('proxy', {group:'dd'});
``````````

target告诉用户，他应该把上边的proxy放到哪里

``````````
var target = new Ext.dd.DDTarget('target', 'dd');
``````````

注意到两者之中相同的dd了吗？这是个分组标志，咱们通过这个限制用户不会像第一节那样，把proxy乱扔到任何地方。只有相同组名的目的地才能接收它，好比你只能把超市货架上的商品放进篮子，而不能满地乱扔一样。

不过这也仅仅是拖拽而已，没有任何效果不是很不爽吗？让我们加入一些小技巧吧，可以让拖拽显得更神奇一些。

``````````
proxy.afterDragDrop = function(target, e, id) {
    var destEl = Ext.get(id);
    var srcEl = Ext.get(proxy.getEl());
    srcEl.insertBefore(destEl);
};
``````````

这个函数是说，在dragdrop之后，会执行这个函数，然后通过id获得target，根据proxy.getEl()获得proxy，然后把proxy添加到target的前头，看起来是放在里边了。

好，脚本都组织好了，打开页面看看效果吧。

![07-03.png][]

当然，为了让画面花花绿绿的，咱们加了不少css样式，稍微给你们看一下吧，省得那些不愿意交钱的人说咱们的截图是用photoshop画的。

``````````
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=gbk">
        <title>dd</title>
        <link rel="stylesheet" type="text/css" href="../../../resources/css/ext-all.css" />
        <script type="text/javascript" src="../../adapter/ext/ext-base.js"></script>
        <script type="text/javascript" src="../../ext-all.js"></script>
        <style type="text/css">
HR {
    clear: both;
    visibility: hidden
}
.block {
    border: 1px red solid;
    margin: 10px;
    min-height: 80px;
}
.item {
    border: 1px green solid;
    background: green;
    float: left;
    margin:10px;
    width: 50px;
    min-height: 50px;
    text-align: center;
}
        </style>
        <script type="text/javascript">
Ext.onReady(function(){

    var proxy = new Ext.dd.DragSource('proxy', {group:'dd'});

    proxy.afterDragDrop = function(target, e, id) {
        var destEl = Ext.get(id);
        var srcEl = Ext.get(proxy.getEl());
        srcEl.insertBefore(destEl);
    };

    var target = new Ext.dd.DDTarget('target', 'dd');

});
        </script>
    </head>
    <body>
        <script type="text/javascript" src="../examples.js"></script>
        <div id="proxy" class="item">item</div>
        <hr />
        <div id="target" class="block">
            <hr />
        </div>
    </body>
</html>
``````````

好了，其实你也可以在lingo-sample/1.1.1/07-02.html看到咱们的例子。很高兴的是，lingo-sample/2.0/07-02.html跟它完全一样。

## 7.4. 再拖！再拖拖。 ##


[07-01.png]: http://203.93.254.59:8889/extdoc/shared/images/07-01.png
[07-02.png]: http://203.93.254.59:8889/extdoc/shared/images/07-02.png
[07-03.png]: http://203.93.254.59:8889/extdoc/shared/images/07-03.png