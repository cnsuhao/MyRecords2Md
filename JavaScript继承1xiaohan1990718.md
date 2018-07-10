---
title: JavaScript继承1
date: 2012-10-10 21:54:00
categories: "开发"
tags:
	- Java

---

今天写个简单的，算是我计划的开始吧~纯手敲，错误难免~~![微笑][QJVY-NZEN-YNZ2.gif]

//function的继承--不是通用的，简单写写，之后会写这类很多。

var US = \{

extend:function(funChild,funParent)\{

var c = funChild.prototype;

var p = funParent.prototype;

var i;

for(i in p)\{

c\[i\] = p\[i\];

\}

\}

\}

var testA = function()\{\};

testA.prototype = \{

name:'Hank',
age:18,

sayName:function()\{

alert("testA:"+this.name);

\},

showAge:function()\{

alert("testA:"+this.age);

\}

\};

var testB = function()\{\};

testB.prototype = \{

age:22,

showAge:function()\{

alert("testB:"+this.age);

\}

\};

US.extend(testB,testA);

var tb = new testB();

tb.sayName();//testA:Hank

tb.sayAge();//22

\---------------------------------------------------------------------------------------------------------------------------------------------------

明天将其他继承陆续整整~~





[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif