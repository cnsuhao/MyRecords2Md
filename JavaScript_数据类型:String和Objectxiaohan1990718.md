---
title: JavaScript_数据类型:String和Object
date: 2012-10-17 20:44:03
categories: "开发"
tags:
	- Java

---

String类型的，没什么好说的。//草。。。输入法成狗屎了。。。。waiting.....download sogou\_pinyin\_XXXX . I cann't write chinese......................................

// today , xiu xian.....................................


说说Object：ECMAScript 中的对象其实是一组数据和功能的集合。

var obj = new Object(\{

name:'Hank',

day:\['sunDay','Sta.','Mon.'\],

items:\[\{

name:'One',

value:1

\},\{

name:'Two',

value:2

\}\],

sayHello:function()\{

alert("hello");

\}

\});

var o = \{\};

var o2 = new Object();

var o3 = object;//以上三种均是有效的：

Object的每个实例都具有下列属性和方法

constructor:保存着用于创建当前对象的函数//重新安装了输入法。。。。

意为：构造函数

hasOwnProperty(propertyName):用于检查给定的属性是否在当前对象实例中。

isPrototypeOf(object):用于检查传入的对象是否是另一个对象的原型。

propertyIsEnumerable(propertyName):用于检查给定的属性是否能够使用for-in语句。

toString():返回对象的字符串表示。

valueOf()：返回对象的字符串、数值或布尔值表示。

由于在ECMAScript中Object是所有对象的基础，因此所有对象具有这些基本的属性和方法。

\---------------------------------------------------------------割-------------------------------------------------------------

下章是操作符、语句。因为今日忙，没来得及总结。估计得过两天。。。。



