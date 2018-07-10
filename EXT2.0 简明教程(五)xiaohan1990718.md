---
title: EXT2.0 简明教程(五)
date: 2012-02-09 19:22:04
categories: "开发"
tags:
	- EXT

---

## 5.1. 呵呵~跳出来和缩回去总给人惊艳的感觉。 ##

浏览器原声的alert()，confirm()，prompt()显得如此寒酸，而且还不能灵活配置，比如啥时候想加个按钮，删个按钮，或者改改按下按钮触发的事件了，都是难上加难的事情。

既然如此，为何不同ext提供的对话框呢？那么漂亮，那么好配置，可以拖啊，可以随便放什么东西，在里边用啥控件都可以，甚至放几个tab乱切换呀，连最小化窗口的功能都提供了。哈哈，神奇啊，完全可以让alert退役了。

## 5.2. 先看看最基本的三个例子 ##

嘿嘿，为了加深认识，还是先去看看examples下的例子吧。1.x在dialog目录下。2.0在message-box目录下。

### 5.2.1. Ext.MessageBox.alert() ###

``````````
Ext.MessageBox.alert('标题', '内容', function(btn) {
    alert('你刚刚点击了 ' + btn);
});
``````````

![05-01.png][]

现在可以通过第一个参数修改窗口的标题，第二个参数决定窗口的的内容，第三个参数是你关闭按钮之后（无论是点ok按钮还是右上角那个负责关闭的小叉叉），就会执行的函数，嘿嘿，传说中的回调函数。

### 5.2.2. Ext.MessageBox.confirm() ###

``````````
Ext.MessageBox.confirm('选择框', '你到底是选择yes还是no？', function(btn) {
    alert('你刚刚点击了 ' + btn);
});
``````````

![05-02.png][]

选择yes或者是no，然后回调函数里可以知道你到底是选择了哪个东东。

### 5.2.3. Ext.MessageBox.prompt() ###

``````````
Ext.MessageBox.prompt('输入框', '随便输入一些东西', function(btn, text) {
    alert('你刚刚点击了 ' + btn + '，刚刚输入了 ' + text);
});
``````````

![05-03.png][]

随便输入几个字，然后点按钮，它会告诉你输入了些什么东西

## 5.3. 如果你想的话，可以控制得更多 ##

### 5.3.1. 可以输入多行的输入框 ###

``````````
Ext.MessageBox.show({
    title: '多行输入框',
    msg: '你可以输入好几行',
    width:300,
    buttons: Ext.MessageBox.OKCANCEL,
    multiline: true,
    fn: function(btn, text) {
        alert('你刚刚点击了 ' + btn + '，刚刚输入了 ' + text);
    }
});
``````````

![05-04.png][]

其实只需要show，我们就可以构造各种各样的窗口了，title代表标题，msg代表输出的内容，buttons是显示按钮，multiline告诉我们可以输入好几行，最后用fn这个回调函数接受我们想要得到的结果。

### 5.3.2. 再看一个例子呗 ###

可能让我们对show这个方法的理解更深

``````````
Ext.MessageBox.show({
    title:'随便按个按钮',
    msg: '从三个按钮里随便选择一个',
    buttons: Ext.MessageBox.YESNOCANCEL,
    fn: function(btn) {
        alert('你刚刚点击了 ' + btn);
    }
});
``````````

![05-05.png][]

我相信buttons这个参数是一个数组，里边的这个参数绝对了应该显示哪些按钮，Ext.MessageBox给我们提供了一些预先定义好的组合，比如YESNOCANCEL,OKCANCEL，可以直接使用。

### 5.3.3. 下一个例子是进度条 ###

实际上只需要将progress这个属性设置为true，对话框里就会显示个条条。

``````````
Ext.MessageBox.show({
    title: '请等待',
    msg: '读取数据中',
    width:240,
    progress:true,
    closable:false
});
``````````

![05-06.png][]

看到进度条了吧，不过它可不会自动滚啊滚的，你需要调用Ext.MessageBox.updateProgress让进度条发生变化。

另外多说一句，closable: false会隐藏对话框右上角的小叉叉，这样咱们就不能随便关掉它了。

现在让咱们加上更新进度条的函数，使用timeout定时更新，这样咱们就可以看到效果了。呵呵~效果真不错，这样咱们以后就可以使用进度条了。

``````````
var f = function(v){
    return function(){
        if(v == 11){
            Ext.MessageBox.hide();
        }else{
            Ext.MessageBox.updateProgress(v/10, '正在读取第 ' + v + ' 个，一共10个。');
        }
   };
};
for(var i = 1; i < 12; i++){
   setTimeout(f(i), i*1000);
}
``````````

### 5.3.4. 动画效果，跳出来，缩回去 ###

超炫效果，让对话框好像是从一个按钮跳出来的，关闭的时候还会自己缩回去。你可以看到它从小变大，又从大变小，最后不见了。实际上的配置缺非常简单，加一个animEl吧。让我们看看上边那个三个按钮的例子会变成什么样子。

``````````
Ext.MessageBox.show({
    title:'随便按个按钮',
    msg: '从三个按钮里随便选择一个',
    buttons: Ext.MessageBox.YESNOCANCEL,
    fn: function(btn) {
        alert('你刚刚点击了 ' + btn);
    },
    animEl: 'dialog'
});
``````````

animEl的值是一个字符串，它对应着html里一个元素的id，比如<div id="dialog"></div>。指定好了这个，咱们的对话框才知道根据哪个元素播放展开和关闭的动画呀。

只需要这样，咱们就得到动画效果，嘿嘿，截不到动画效果的图，大家自己去看吧。

以上的例子在examples里都可以找到，不过咱们也提供了一份自己的例子，1.x在lingo-sample/1.1.1/05-01.html。2.0在lingo-sample/2.0/05-01.html。

好消息是，这部分的api没有什么改动。不过表现形式上有些差别，如果像我在例子里写的那样，一次生成N个MessageBox，只能显示最后一个对话框。

不过在1.x里明显有一些数据同步的问题，1.x里的updateProgress甚至可以影响其他对话框的msg，以及可以关闭最后那个对话框。2.0里至少是好的。

## 5.4. 让弹出窗口，显示我们想要的东东，比如表格 ##

2.0需要window来完成这个任务，1.x版的BasicDialog稍后加上。

### 5.4.1. 2.0的弹出表格哦 ###

稍微说一下window咋用呢？其实看起来跟MessageBox差不多啦，只是可以在里边随便放东西，现在先看个单纯的例子。

``````````
var win = new Ext.Window({
    el:'window-win',
    layout:'fit',
    width:500,
    height:300,
    closeAction:'hide',

    items: [{}],

    buttons: [{
        text:'按钮'
    }]
});
win.show();
``````````

首先要讲明的是，这个window需要一个对应的div呀，就像el对应的'window-win'一样，这个div的id就应该等于'window-win'，然后设置宽和高，这些都很明朗。

其次，需要设置的是布局类型，layout:'fit'说明布局会适应整个window的大小，估计改变大小的时候也要做响应的改变。

closeAction:'hide'是一个预设值，简单来说就是你用鼠标点了右上角的叉叉，会执行什么操作，这里就是隐藏啦。问为啥是隐藏？因为，因为预设啦，乖，背下来撒。

items部分，嘿嘿~就是告诉咱们的window里要有什么内容啦。这里放表格，放树形，吼吼。

buttons里设置在底端显示的按钮。我们就为了试一下，弄了一个按钮，但是按了没反应，嘿嘿。

最后调用一下show()，让窗口显示出来。

看一下截图啦，更直观。

![05-07.png][]

中间的空白就是items:\[\{\}\]的杰作，默认\{\}会成为一个Ext.Panel，咱们什么都没定义，里边自然什么都没有。当然500\*300不会只有这么大，但是为了让图片小一点儿，我把它拖下了，嘿嘿~自动支持的修改大小效果，帅吧？

例子超简单，见lingo-sample/2.0/05-02.html。

### 5.4.2. 向2.0的window里加表格 ###

唉，地方都划出来了，弄个表格放进去就好了呗。

首先弄一个grid，超简单那种。我是直接把第二章的例子给copy了过来，嘿嘿，表格还是那个表格哟。

有了表格，直接扔到window里，然后ok，哈哈~效果如下：

![05-08.png][]

看到没？表格出来了，如果想加分页条什么的，只管动手，别怕伤到窗口。

现在回头让我们看看，需要注意些什么。

1.  第一，grid不用调用render()了，只要加入了window，在win.show()的时候，会自动渲染里边的组件。
2.  第二，html里，要把grid对应的div写到window的div里面，嵌套关系。
    
    ``````````
    <div id="window-win">
        <div id="grid"></div>
    </div>
    ``````````
3.  第三，如果还不知道怎么把grid放进window里，我给你看下代码。
    
    ``````````
    var win = new Ext.Window({
        el:'window-win',
        layout:'fit',
        width:500,
        height:300,
        closeAction:'hide',
    
        items: [grid],
    
        buttons: [{
            text:'按钮'
        }]
    });
    ``````````
    
    看到items:\[grid\]了吗？就这么简单哟。

好了，具体例子在lingo-sample/2.0/05-03.html。敬请大家继续关注。

## 5.5. 更进一步撒。 ##


[05-01.png]: http://203.93.254.59:8889/extdoc/shared/images/05-01.png
[05-02.png]: http://203.93.254.59:8889/extdoc/shared/images/05-02.png
[05-03.png]: http://203.93.254.59:8889/extdoc/shared/images/05-03.png
[05-04.png]: http://203.93.254.59:8889/extdoc/shared/images/05-04.png
[05-05.png]: http://203.93.254.59:8889/extdoc/shared/images/05-05.png
[05-06.png]: http://203.93.254.59:8889/extdoc/shared/images/05-06.png
[05-07.png]: http://203.93.254.59:8889/extdoc/shared/images/05-07.png
[05-08.png]: http://203.93.254.59:8889/extdoc/shared/images/05-08.png