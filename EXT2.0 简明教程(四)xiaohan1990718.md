---
title: EXT2.0 简明教程(四)
date: 2012-02-09 19:21:26
categories: "开发"
tags:
	- EXT

---

Ext的表单和输入控件

## 4.1. 不用ext的form啊，不怕错过有趣的东西吗？ ##

初看那些输入控件，其实就是修改了css样式表而已。你打开firebug看看dom，确实也是如此，从这点看来，似乎没有刻意去使用ext的必要，诚然，如果单单要一个输入框，不管添入什么数据，就点击发送到后台，的确是不需要ext呢。

你不想用一些默认的数据校验吗？你不想在数据校验失败的时候，有一些突出的提示效果吗？你不想要超炫的下拉列表combox吗？你不想要一些你做梦才能朦胧看到的选择控件吗？唉，要是你也像我一样禁不起诱惑，劝你还是随着欲望的节拍，试一下ext的form和输入控件。

## 4.2. 慢慢来，先建一个form再说 ##

``````````
var form = new Ext.form.Form({
    labelAlign: 'right',
    labelWidth: 50
});
form.add(new Ext.form.TextField({
    fieldLabel: '文本框'
}));
form.addButton("按钮");
form.render("form");
``````````

![04-01.png][]

简单来说，就是构造了一个form，然后在里边放一个TextField，再放一个按钮，最后执行渲染命令，在id="form"的地方画出form和里边包含的所有输入框和按钮来。刷拉一下就都出来了。

不过即使这样，圆角边框可不是form自带的，稍稍做一下处理，参见html里的写法。

``````````
<div style="width:220px;margin-left:0px;">
    <div class="x-box-tl"><div class="x-box-tr"><div class="x-box-tc"></div></div></div>
    <div class="x-box-ml"><div class="x-box-mr"><div class="x-box-mc">
        <h3 style="margin-bottom:5px;">form</h3>
        <div id="form"></div>
    </div></div></div>
    <div class="x-box-bl"><div class="x-box-br"><div class="x-box-bc"></div></div></div>
</div>
<div class="x-form-clear"></div>
``````````

开头结尾那些div就是建立圆角的，有了这些我们都可以在任何地方使用这种圆角形式了，不限于form哟。

html例子，1.x在lingo-sample/1.1.1/04-01.html

2.0里的FormPanel跟1.x里已经基本完全不一样了，咱们先看个简单例子：

![04-01a.png][]

代码如下：

``````````
var form = new Ext.form.FormPanel({
    defaultType: 'textfield',
    labelAlign: 'right',
    title: 'form',
    labelWidth: 50,
    frame: true,
    width: 220,

    items: [{
        fieldLabel: '文本框'
    }],
    buttons: [{
        text: '按钮'
    }]
});
form.render("form");
``````````

html里不需要那么多东西了，只需要定义一个div id="form"就可以实现这一切。明显可以感觉到初始配置更紧凑，利用items和buttons指定包含的控件和按钮。

现在先感觉一下，我们后面再仔细研究，例子见lingo-sample/2.0/04-01.html。

## 4.3. 胡乱扫一下输入控件 ##

兄弟们应该都有html开发的经验了，像什么input用的不在少数了，所以咱们在这里也不必浪费唾沫，大概扫两眼也知道ext的输入控件是做什么的。

1.  像TextField，TextArea，NumberField这类当然是可以随便输入的了。
2.  ComboBox，DateField继承自TriggerField。他们长相差不多，都是一个输入框右边带一个图片，点击以后就跳出一大堆东西来让你选择，输入框里头显示的是你选中的东西。
3.  Checkbox和Radio，ext没有过多封装，基本上还是原来的方式。
4.  Button，这个东东其实就是一个好看的div，跟comboBox一样，不是对原有组件的美化，而是重新做的轮子。你可以选择用以前那种难看的type="button"，还是用咱们漂亮的div，看你的爱好了。type="submit"和type="reset"也一样没有对应的组件，都使用Button好了。
5.  文件上传框，type="file"，因为浏览器的安全策略，想上传文件，必须使用type="file"，而且我们不能使用js修改上传框的值，所以非常郁闷，目前的方式是把它隐藏起来，然后在点击咱们漂亮的Button时，触发上传框的点击事件，从而达到上传的目的。在这方面extjs.com论坛上有不少实现上传的扩展控件，咱们可以参考一下。

## 4.4. 起点高撒，从comboBox往上蹦。 ##

我觉得像TextField啊，TextArea啊，都是在原来的东西上随便加了几笔css弄出来的，大家都会用，所以没什么大搞头，最后综合起来一说就ok了。而这个comboBox跟原有的select一点儿关系都没有，完全是用div重写画的。所以，嘿嘿~

耳听为虚，眼见为实，先看看所谓的comboBox究竟是个什么模样。

![04-02.png][]

漂亮不？漂亮不？怎么看都比原生select强哟。啦啦啦，咱们看看代码撒。不过呢，comboBox支持两种构造方式，一一看来。

### 4.4.1. 凭空变出个comboBox来。 ###

>

### 4.4.2. 把select变成comboBox。 ###

### 4.4.3. 破例研究下comboBox的内在本质哟 ###

### 4.4.4. 嘿嘿~本地的做完了，试试远程滴。 ###

### 4.4.5. 给咱们的comboBox安上零配件 ###

### 4.4.6. 每次你选择什么，我都知道 ###

### 4.4.7. 露一小手，组合上面所知，省市县三级级联。哈哈~ ###

这是一个相当典型的案例，以前经常用select做，现在用comboBox实现的效果又会如何呢？

#### 4.4.7.1. 先做一个模拟的，所有数据都在本地 ####

#### 4.4.7.2. 再做一个有后台的，需要放在服务器上咯 ####

## 4.5. 还要做，字段验证呀，表单提交啊，表单布局咯，文件上传哟 ##


[04-01.png]: http://203.93.254.59:8889/extdoc/shared/images/04-01.png
[04-01a.png]: http://203.93.254.59:8889/extdoc/shared/images/04-01a.png
[04-02.png]: http://203.93.254.59:8889/extdoc/shared/images/04-02.png