---
title: EXT2.0 简明教程(八)
date: 2012-02-09 19:23:56
categories: "开发"
tags:
	- EXT

---

## 8.1. Ext.get ##

ext里用来获得Element的一个函数，用途还算比较广，可以通过不少途径获得咱们需要的Element，而这个Element包括很多有趣的功能。

Element跟document.getElementById("myDiv")得到的dom对象是不一样的，虽然你还可以使用老方式获得指定id的元素，但那样就失去了ext提供的各种常用操作，动画啦，定位啦，css啦，事件啦，拖拽啦。其实也不用担心，即便使用了Ext.get()获得了myDiv，还是可以直接访问document.getElementById()应该得到的部分，而且挺简单的，Ext.get().dom就可以了。

下面让偶们来看看这些基本的功能会是咋样呢？

1.  先获得一个Element
    
    ``````````
    var myDiv = Ext.get('myDiv');
    ``````````
    
    这里我们传入的是一个id，你可以在html里看到<div id="myDiv"></div>，然后我们用Ext.get('myDiv')从html里取得这个div，然后封装成Element对象，现在这个对象就已经放到缓存中了，以后再用的时候就更快撒。
2.  最吸引眼球的是动画效果，所以我们先动两下。
    
    ``````````
    myDiv.hightlight();
    ``````````
    
    红色高亮，然后渐退。
    
    ``````````
    myDiv.addClass('red');
    ``````````
    
    添加自定义CSS类，css里有.red \{background: red;\}的定义，这样myDiv的背景直接变成了红色。
    
    ``````````
    myDiv.center();
    ``````````
    
    myDiv移动的窗口中间，包括垂直和竖直居中。
    
    ``````````
    myDiv.setOpacity(.25);
    ``````````
    
    使myDiv半透明
3.  再看看怎么才好渐变动画
    
    ``````````
    myDiv.setWidth(100);
    ``````````
    
    这样可以直接设置myDiv的宽度，是没有渐变动画的。
    
    ``````````
    myDiv.setWidth(100, true);
    ``````````
    
    这样就打开了动画开关，如此简单就可以看到myDiv在动咯。
    
    咱们还可以控制动画的动作，如下
    
    ``````````
    myDiv.setWidth(100, {
        duration: 2,
        callback: this.highlight,
        scope: this
    });
    ``````````
    
    duration是间隔，数字越大移动越慢，callback说是动画完成后执行，但我没饰演出来，scope是callback执行的范围。

动画没法截图，还是看看lingo-sample/1.1.1/08-01.html，lingo-sample/2.0/08-01.html吧，四个按钮可以让myDiv在窗口里乱动，哈哈。

## 8.2. 要是我们想一下子获得一堆元素咋办？ ##

现在像css那样的批量选择方式真的很流行，ext里也没有落伍，一定会赶这个潮流。

1.  选择所有<P>元素
    
    现在我们要获得所有<P>元素，然后让他们都闪一下。
    
    ``````````
    Ext.select("p").highlight();
    ``````````
2.  按照css的class选择
    
    首先我们有几个div，都使用class="red"，然后我们让他们都闪一下，嘿嘿嘿~因为highlight()调用比较简单嘛。
    
    ``````````
    Ext.select("div.red").highlight();
    ``````````

这种方式在prototype和jquery里已经发扬光大，而且还光大得很呢，你只需要按照css的选择方式，就可以得到你需要的集合。这方面其实jquery颇为神奇，把select用的真是出神入化，可叹，它对js封装太狠，你用jquery的时候完全感觉不到自己是在用javascript，这样接触原生方法的机会很少，等于把自己绑定到jquery上，最后权衡利弊，只好忍痛割爱了。

批量选择，见lingo-sample/1.1.1/08-02.html和lingo-sample/2.0/08-02.html。

## 8.3. DomHelper和Template动态生成html ##

用dom生成html元素一直是头疼的事情，以前都是听springside的教导，使用jsTemplate和Scriptaculous的组合。现在到了ext里面，我们就来看看它自己的实现。

### 8.3.1. DomHelper用来生成小片段 ###

使用DomHelper非常灵活，超简单就可以生成各种html片段，遇到复杂情况也要求助于它。

大概就是这么用

``````````
var list = Ext.DomHelper.append('parent', {tag: 'div', cls: 'red'});
``````````

它就是向id=parent这个元素里，添加一个div元素。

按照文档里讲的，第二个参数\{\}里，除了四个特殊属性以外都会复制给新生成元素的属性，这四个特殊属性是

1.  tag，告诉我们要生成一个什么标签，div啦，span啦，诸如此类。
    
    千万别告诉我到现在你还不知道这些html标签，中间告诉你多少次先去学学html和css啦？你飞过来的不成？
2.  cls，指的是<div class="red"></div>这种标签中的class属性，因为class是关键字，正常情况下应该写成className，可jack说className太长了，最后就变成cls了。-\_-。
    
    他就喜欢玩这个，把dataStore写成ds，DomHelper写成dh，Element写成el，ColumnModel写成cm，SelectionModel是sm。唉，发明的专业名词缩写好多呀。
3.  children，用来指定子节点，它的值是一个数组，里边包含了更多节点。
4.  html，对应innerHTML，觉得用children描述太烦琐，直接告诉节点里边的html内容也是一样。

DomHelper除了append还有几个方法，指定将新节点添加到什么位置。

为了比对效果，先放一个初始页面。

![08-01.png][]

原始的html是这样的。一个div下有4个节点，其中第三个子节点下还有自己的子节点。

``````````
<div id="parent" style="border: 1px solid black;padding: 5px;margin: 5px;background: lightgray;">
  <p id="child1">child1</p>
  <p id="child2">child2</p>
  <div id="child3">
    <p id="child5">inner child</p>
  </div>
  <p id="child3">child4</p>
</div>
``````````

1.  append是将新节点放到指定节点的最后。
    
    ``````````
    Ext.DomHelper.append('parent', {tag: 'p', cls: 'red', html: 'append child'});
    ``````````
    
    ![08-02.png][]
2.  insertBefore，新节点插入到指定节点前面。
    
    ``````````
    Ext.DomHelper.insertBefore('parent', {tag: 'p', cls: 'red', html: 'insert before child'})
    ``````````
    
    ![08-03.png][]
3.  insertAfter，新节点插入到指定节点后面。
    
    ``````````
    Ext.DomHelper.insertAfter('parent', {tag: 'p', cls: 'red', html: 'insert before child'})
    ``````````
    
    ![08-04.png][]
4.  overwrite，会替换指定节点的innerHTML内容。
    
    ``````````
    Ext.DomHelper.overwrite('child3', {tag: 'p', cls: 'red', html: 'overwrite child'})
    ``````````
    
    ![08-05.png][]

闲来无聊，也看一看children这个属性的用法。

``````````
Ext.DomHelper.append('parent', {
    tag: 'ul',
    style: 'background: white;list-style-type: disc;padding: 20px;',
    children: [
        {tag: 'li', html: 'li1'},
        {tag: 'li', html: 'li2'},
        {tag: 'li', html: 'li3'}
    ]
});
``````````

这样就在parent里添加了一个ul标签，ul里包含三个li。呵呵~炫啊。

![08-06.png][]

代码见lingo-sample/1.1.1/08-03.html和lingo-sample/2.0/08-03.html。

### 8.3.2. 批量生成还是需要Template模板 ###

场景模拟：目前有三男两女的json数据，要输出成html显示出来。

``````````
var data = [
    ['1','male','name1','descn1'],
    ['2','female','name2','descn2'],
    ['3','male','name3','descn3'],
    ['4','female','name4','descn4'],
    ['5','male','name5','descn5']
];
``````````

照搬grid时的测试数据呢，嘿嘿。只不过这次我们用的不再是ds，cm，grid的方式解析输出，而是用模板自己定义输出的格式。

首先要定义一个模板

``````````
var t = new Ext.Template(
    '<tr>',
        '<td>{0}</td>',
        '<td>{1}</td>',
        '<td>{2}</td>',
        '<td>{3}</td>',
    '</tr>'
);
t.compile();
``````````

索引从0开始，一共4个元素。然后在用的时候，这样子。

``````````
for (var i = 0; i < data.length; i++) {
    t.append('some-element', data[]);
}
``````````

这段代码对应html中的一个表格，id="some-element"是tbody的id，我们使用模板为table增添了四行。

``````````
<table border="1">
    <tbody id="some-element">
        <tr>
            <td>id</td>
            <td>sex</td>
            <td>name</td>
            <td>descn</td>
        </tr>
    </tbody>
</table>
``````````

最终的显示结果就是包含五行数据的表格：

![08-07.png][]

定义模板的时候，可以使用Ext.util.Format里的工具方法，对数据进行格式化。常用的就是trim去掉收尾空格和ellipsis(10)，ellipsis判断，当字符长度超过10时，自动截断字符串并在末尾添加省略号，很常用的功能哩。

在模板里使用这些函数的话也很简单，不过我不说，你还是不知道，嘿嘿

``````````
var t = new Ext.Template(
    '<tr>',
        '<td>{0}</td>',
        '<td>{1:trim}</td>',
        '<td>{2:trim}</td>',
        '<td>{3:ellipsis(10)}</td>',
    '</tr>'
);
t.compile();
``````````

如此这般，冒号加函数名称就可以实现我们的愿望了。

可惜人算终不如天算，jack再神奇，也不可能考虑到所有的可能性，比如现在我们就想根据性别不同显示图片，jack怕是想破了脑袋，也想不出这种可能来，所以呢，他干脆不想了，而是给咱们留了一个自定义函数的接口。

``````````
var t = new Ext.Template(
    '<tr>',
        '<td>{0}</td>',
        '<td>{1:this.renderSex}</td>',
        '<td>{2:trim}</td>',
        '<td>{3:ellipsis(10)}</td>',
    '</tr>'
);
t.renderSex = function(value) {
    if (value == 'male') {
        return "<span style='color:red;font-weight:bold;'>红男</span><img src='user_male.png' />";
    } else {
        return "<span style='color:green;font-weight:bold;'>绿女</span><img src='user_female.png' />";
    }
};
t.compile();
``````````

显示的红男绿女，就像我们预想的那样呈现在我们眼前了。


[08-01.png]: http://203.93.254.59:8889/extdoc/shared/images/08-01.png
[08-02.png]: http://203.93.254.59:8889/extdoc/shared/images/08-02.png
[08-03.png]: http://203.93.254.59:8889/extdoc/shared/images/08-03.png
[08-04.png]: http://203.93.254.59:8889/extdoc/shared/images/08-04.png
[08-05.png]: http://203.93.254.59:8889/extdoc/shared/images/08-05.png
[08-06.png]: http://203.93.254.59:8889/extdoc/shared/images/08-06.png
[08-07.png]: http://203.93.254.59:8889/extdoc/shared/images/08-07.png