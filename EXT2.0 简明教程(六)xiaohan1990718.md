---
title: EXT2.0 简明教程(六)
date: 2012-02-09 19:22:42
categories: "开发"
tags:
	- EXT

---

不同浏览器的兼容

## 第 6 章 奔腾吧！让不同的浏览器里显示一样的布局。 ##

周三, 12/12/2007 - 15:00 — liki

42

vote

## 6.1. 有了它，我们就可以摆脱那些自称ui设计师的人了。 ##

对布局很是不熟，至今为止，也是一直在抄土豆demo里的BorderLayout，frank的deepcms ProjectTracker里的ViewPort布局而已，不过有了布局，咱们不用再去摆弄frameset了，只需要div就可以做成端端正正的布局，嗯，只这一点儿就吸引了多少眼球啊。

唉，咱们一起学学关于布局的用法吧。

## 6.2. 关于BorderLayout ##

理论上说，把整个窗口切成五块就够了吧？东南西北中，east,south,west,north,center其中只有center中间这个部分是必须的，你完全可以把围绕在它四周的东西当作配角。

这样说还是太抽象，这个时候效果图绝对比其他途径来的直观。

![06-01.png][]

实际上代码还是比较干净的。

``````````
var mainLayout = new Ext.BorderLayout(document.body, {
    north: {
        initialSize: 50
    }, south: {
        initialSize: 50
    }, east: {
        initialSize: 100
    }, west: {
        initialSize: 100
    }, center: {
    }
});

mainLayout.beginUpdate();
mainLayout.add('north', new Ext.ContentPanel('north-div', {
    fitToFrame: true, closable: false
}));
mainLayout.add('south', new Ext.ContentPanel('south-div', {
    fitToFrame: true, closable: false
}));
mainLayout.add('east', new Ext.ContentPanel('east-div', {
    fitToFrame: true, closable: false
}));
mainLayout.add('west', new Ext.ContentPanel('west-div', {
    fitToFrame: true, closable: false
}));
mainLayout.add('center', new Ext.ContentPanel('center-div', {
    fitToFrame: true
}));
mainLayout.endUpdate();
``````````

html需要五个div与其对应，div与ContentPanel是一一对应的，请看他们的id。

``````````
<div id="north-div">north</div>
<div id="south-div">south</div>
<div id="east-div">east</div>
<div id="west-div">west</div>
<div id="center-div">center</div>
``````````

这个其实挺有意思的，你必须先构造一个BorderLayout，指定需要渲染的部分，这里是document.body，并指定5个部分的初始化大小，然后调用beginUpdate()让整个布局先不要刷新，当然我们最后会调用endUpdate()刷新布局，这样用户就获得了更好的体验。

beginUpdate()之后，我们立刻使用add方法，向5个部分分别加入Ext.ContentPanel，这些面板的第一个参数是对应dom的id，后边是附加的参数，比如fitToFrame:true，它告诉面板在布局区域改变大小的时候调整自己的大小，然后是closable:false，这样用户就不能点击关闭按钮，关闭这个面板。

好了，你也看到了，这五个部分明显已经分隔开了，使用的时候我们只需要在合适的地方放上合适的东西就行了。

例子在lingo-sample/1.1.1/06-01.html。

## 6.3. 嗯，不如再看看附加效果 ##

其实，即使只能在不同浏览器，把一个窗口切成相同的部分，也是足够了，不过ext带给我们的不仅仅是如此，让我们再看看其他部分吧。

### 6.3.1. 先看看split ###

``````````
var mainLayout = new Ext.BorderLayout(document.body, {
    north: {
        initialSize: 50,
        split: true
    }, south: {
        initialSize: 50,
        split: true
    }, east: {
        initialSize: 100,
        split: true
    }, west: {
        initialSize: 100,
        split: true
    }, center: {
    }
});
``````````

让我们给所有区域都加上split:true，看看会有什么效果？

![06-02.png][]

请注意一点，这并不仅仅是那些边框变粗了，split让我们可以自由拖动边框，让用户可以改变各个区域的大小。

当然，我们不会让用户为所欲为的，让我们加上一点点限制，这点儿限制绝对不会让用户感到难堪。

``````````
var mainLayout = new Ext.BorderLayout(document.body, {
    north: {
        initialSize: 50,
        minSize: 40,
        maxSize: 60,
        split: true
    }, south: {
        initialSize: 50,
        minSize: 40,
        maxSize: 60,
        split: true
    }, east: {
        initialSize: 100,
        minSize: 80,
        maxSize: 120,
        split: true
    }, west: {
        initialSize: 100,
        minSize: 80,
        maxSize: 120,
        split: true
    }, center: {
    }
});
``````````

minSize和maxSize让用户只能在我们决定的范围内修改区域的大小，既不会太大，也不会太小。用户的行为受限，也减少了他们抱怨的机会。嘿嘿，一切尽在掌握中。

例子见lingo-sample/1.1.1/06-02.html

### 6.3.2. 再试试titlebar ###

``````````
var mainLayout = new Ext.BorderLayout(document.body, {
    north: {
        initialSize: 50,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        split: true
    }, south: {
        initialSize: 50,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        split: true
    }, east: {
        initialSize: 100,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        split: true
    }, west: {
        initialSize: 100,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        split: true
    }, center: {
    }
});
``````````

在上面的基础上，我们加入了titlebar，然后看到的就是这幅情景。

![06-03.png][]

标题栏是空的，要加标题我们另有地方，看看ContentPanel的部分。

``````````
mainLayout.beginUpdate();
mainLayout.add('north', new Ext.ContentPanel('north-div', {
    fitToFrame: true, closable: false, title: '北'
}));
mainLayout.add('south', new Ext.ContentPanel('south-div', {
    fitToFrame: true, closable: false, title: '南'
}));
mainLayout.add('east', new Ext.ContentPanel('east-div', {
    fitToFrame: true, closable: false, title: '东'
}));
mainLayout.add('west', new Ext.ContentPanel('west-div', {
    fitToFrame: true, closable: false, title: '西'
}));
mainLayout.add('center', new Ext.ContentPanel('center-div', {
    fitToFrame: true
}));
mainLayout.endUpdate();
``````````

经过这些改变，整个布局就变成了这个样子。

![06-04.png][]

例子是lingo-sample/1.1.1/06-03.html。

### 6.3.3. 还不够，还不够，让四周的区域可以缩起来 ###

很多软件都可以这样哦，看小面板不顺眼，就折叠起来，为中间的工作区留出更多空间哟。就像这样。

![06-05.png][]

都折叠上以后就变成这样。

![06-06.png][]

其实只要加一个属性就可以了，看看代码中的collapsible: true造就了现在的盛况。

``````````
var mainLayout = new Ext.BorderLayout(document.body, {
    north: {
        initialSize: 50,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        collapsible: true,
        split: true
    }, south: {
        initialSize: 50,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        collapsible: true,
        split: true
    }, east: {
        initialSize: 100,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        collapsible: true,
        split: true
    }, west: {
        initialSize: 100,
        minSize: 40,
        maxSize: 60,
        titlebar: true,
        collapsible: true,
        split: true
    }, center: {
    }
});
``````````

你还可以加上collapsedTitle属性，让北方和南方区域折叠之后显示，这个属性只在north和south部分有效，因为west和east是垂直的，似乎没有办法让文字旋转90度显示，所以我们需要其他方法。

参考网上的方式，是用一个图片，窄窄高高的图片，然后把它作为对应css样式的背景图，这样在east和west折叠的时候就会显示它们了。让咱们试验一下好了。

我们需要设置的css有两个，west对应左边，east对应右边。

``````````
.x-layout-collapsed-west {
    background-image: url(user_male.png);
    background-repeat: no-repeat;
    background-position: center;
}

.x-layout-collapsed-east {
    background-image: url(user_female.png);
    background-repeat: no-repeat;
    background-position: center;
}
``````````

最后就变成了这样。

![06-07.png][]

嘿嘿，有意思吧。代码都在lingo-sample/1.1.1/06-04.html里呢，你也试试吧。

### 6.3.4. 给这些区域都加上个关闭按钮 ###

``````````
mainLayout.beginUpdate();
mainLayout.add('north', new Ext.ContentPanel('north-div', {
    fitToFrame: true, closable: true, title: '北'
}));
mainLayout.add('south', new Ext.ContentPanel('south-div', {
    fitToFrame: true, closable: true, title: '南'
}));
mainLayout.add('east', new Ext.ContentPanel('east-div', {
    fitToFrame: true, closable: true, title: '东'
}));
mainLayout.add('west', new Ext.ContentPanel('west-div', {
    fitToFrame: true, closable: true, title: '西'
}));
mainLayout.add('center', new Ext.ContentPanel('center-div', {
    fitToFrame: true
}));
mainLayout.endUpdate();
``````````

这个部分跟ContentPanel有关，把参数closable改成true就会出现那个小叉叉，按一下这个区域就关上了。

![06-08.png][]

可惜现在还不知道关闭以后再怎么打开，嘿嘿。

### 6.3.5. 用NestedLayoutPanel在五块中再进行分割，实现更复杂的布局 ###

我不会用。

## 6.4. 2.0的ViewPort是完全不同的实现 ##

简单来说，用了ViewPort摆脱先定义BorderLayout，再beginUpdate，endUpdate的麻烦，我们就问了，为什么事情不能更简单明了呢，就让我们看看用2.0解决上头的五块是个什么样子？

首先html里的东东不变。

``````````
<div id="north-div">north</div>
<div id="south-div">south</div>
<div id="east-div">east</div>
<div id="west-div">west</div>
<div id="center-div">center</div>
``````````

剩下的就是代码了，还是那句话，我们想要在一个地方配置好所有东西，不想东奔西跑，说不定丢了什么也不知道，维护多个地方的配置简直是噩梦，2.0啊，我们崇拜你。

``````````
var viewport = new Ext.Viewport({
    layout:'border',
    items:[{
        title: 'north',
        region: 'north',
        contentEl: 'north-div',
        split: true,
        border: true,
        collapsible: true,
        height: 50,
        minSize: 50,
        maxSize: 120
    },{
        title: 'south',
        region: 'south',
        contentEl: 'south-div',
        split: true,
        border: true,
        collapsible: true,
        height: 50,
        minSize: 50,
        maxSize: 120
    },{
        title: 'east',
        region: 'east',
        contentEl: 'east-div',
        split: true,
        border: true,
        collapsible: true,
        width: 120,
        minSize: 120,
        maxSize: 200
    },{
        title: 'west',
        region: 'west',
        contentEl: 'west-div',
        split: true,
        border: true,
        collapsible: true,
        width: 120,
        minSize: 120,
        maxSize: 200
    },{
        title: 'center',
        region: 'center',
        contentEl: 'center-div',
        split: true,
        border: true,
        collapsible: true
    }]
});
``````````

如果非要挑刺的话，那就是没有closable的选项了，不过现实谁会去关闭一块面板啊？至少我不会滴。

现在所有配置都放在一起了，也不用先创建后布局两步走，方便呀。

![06-09.png][]

## 6.5. 稍稍感叹一下2.0的简洁吧，让人吃惊的还在后头呢。 ##


[06-01.png]: http://203.93.254.59:8889/extdoc/shared/images/06-01.png
[06-02.png]: http://203.93.254.59:8889/extdoc/shared/images/06-02.png
[06-03.png]: http://203.93.254.59:8889/extdoc/shared/images/06-03.png
[06-04.png]: http://203.93.254.59:8889/extdoc/shared/images/06-04.png
[06-05.png]: http://203.93.254.59:8889/extdoc/shared/images/06-05.png
[06-06.png]: http://203.93.254.59:8889/extdoc/shared/images/06-06.png
[06-07.png]: http://203.93.254.59:8889/extdoc/shared/images/06-07.png
[06-08.png]: http://203.93.254.59:8889/extdoc/shared/images/06-08.png
[06-09.png]: http://203.93.254.59:8889/extdoc/shared/images/06-09.png