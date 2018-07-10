---
title: Javascript继承2
date: 2012-10-11 21:26:31
categories: "开发"
tags:
	- Java

---

//纯手敲--错误难免~~![微笑][QJVY-NZEN-YNZ2.gif]

第二种实现继承的方式还行：

//prototype模式的封装函数 的继承方法

function extend(child,parent)\{

var empFn = function()\{\};//利用空对象作为中介

empFn.prototype = parent.prototype;

child.prototype = new empFn();

child.prototype.constructor = child;

\}


var parentTest= function()\{\};

parentTest.prototype = \{

sayName:function()\{

alert("I am Hank.");

\}

\};

var childTest= function()\{\};

//测试该继承方法

extend(childTest,parentTest);//执行该继承函数

//测试child是否实现继承parent的方法

var child\_test = new childTest();

child\_test.sayName();//I am Hank.

说明：这个extend函数,据说是YUI库的实现的方法（应该还要加上一句:child.uber = parent.prototype）,因为本人未去研究该库，所以具体就不知道了。昨天的那个继承是原型链继承方式实现的叫做**拷贝继承**，有很多地方未做判定，本博作为技术交流使用，问题多多，逐步改进。![微笑][QJVY-NZEN-YNZ2.gif]

另献上一个Javascript的练习并方便随时测试使用的网址：http://labs.codecademy.com/\#:workspace



[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif