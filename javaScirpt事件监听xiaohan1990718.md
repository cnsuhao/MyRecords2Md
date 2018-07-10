---
title: javaScirpt事件监听
date: 2013-05-19 21:51:00
categories: "开发"
tags:
	- java

---

这段时间对JS有些想法，对个人职业又有了点思索，这事以后细说，先将今天看到的写下来，继续积累代码先。

js事件：因为一些个人原因，维护了一个事件工具类，方便今后维护使用，代码如下：

``````````
/**
 * @class Base.EventUtil
 * @extends Object
 * 事件工具类
 */
Base.EventUtil = {
    /**
     * 添加事件
     * @param oElement 需添加事件的元素
     * @param sEvent 添加的事件名称
     * @param fnHandler 添加的事件处理方法
     */
    addHandler: function (oElement, sEvent, fnHandler) {
        oElement.addEventListener ? oElement.addEventListener(sEvent, fnHandler, false) : oElement.attachEvent("on" + sEvent, fnHandler)
    },
    /**
     * 移除事件(注意的问题：移除的fnHandler名称要与addHandler进去的fnHanlder名称保持一致)
     * @param oElement 需移除事件的元素
     * @param sEvent 移除的事件名称
     * @param fnHandler 回调处理函数
     */
    removeHandler: function (oElement, sEvent, fnHandler) {
        console.log(oElement[sEvent]);
        oElement.removeEventListener ? oElement.removeEventListener(sEvent, fnHandler, false) : oElement.detachEvent("on" + sEvent, fnHandler)
    },
    //***************************************顺带封装一些常用的事件处理方法***************************************
    /**
     * 单击事件监听
     * @param oElement
     * @param fnHandler
     */
    addClickEvent:function(oElement,fnHandler){
        this.addHandler(oElement,'click',fnHandler);
    },
    /**
     * 页面加载事件
     * @param oElement
     * @param fnHandler
     */
    addLoadEvent:function(oElement,fnHandler){
        this.addHandler(oElement,'load',fnHandler);
    },
    /**
     * 双击事件
     * @param oElement
     * @param fnHandler
     */
    addDbClickEvent:function(oElement,fnHandler){
        this.addHandler(oElement,'dbclick',fnHandler);
    }
};
``````````


以上代码其实源于看到了：view-source:http://fgm.cc/learn/lesson4/08.html 突然感觉到之前自己多么无知和没想法。

测试使用代码：（注意事项部分在注解里写明了）

``````````
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
    <title>事件处理Test</title>
    <script type='text/javascript' src="../../Base-all.js"></script>
    <script type="text/javascript">
        //使用浏览器load事件
        Base.EventUtil.addLoadEvent(this, function () {
            var btn = Base.getByTagName('button')[0];
            Base.EventUtil.addClickEvent(btn, fnHandler);
            function fnHandler() {
                    alert('OK@');
                    Base.EventUtil.removeHandler(btn,'click',fnHandler);
            }
        });
    </script>
</head>
<body>

    <button>按钮</button>

</body>
</html>
``````````


其实我不是很明白为何html原生方法中移除监听的方法函数名称要和添加的一致。在Ext中需要调用移除方法和监听事件名称即可,或许内部处理给封装了吧，暂时还没有细看ext源码。恩，得好好研究下～～

\------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

兴趣是很好的学习兴奋剂～ I like~

