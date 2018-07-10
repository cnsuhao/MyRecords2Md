---
title: Javascript基础知识题答案（前7题--符题目）
date: 2012-11-03 23:03:09
categories: "开发"
tags:
	- Java

---

1、typeof(NaN) 、typeof(Infinity)、typeof(null)、typeof(undefined)//number/number/object/undefined 原因：NaN是number类型中第一个特殊 null是空对象 undefined也是javascript的一个类型之一
2、NaN == NaN //false NaN与任何对象都不相等 但是javascript提供了一个isNaN()的方法来判断。
3、NaN != NaN //
4、NaN >= NaN //这题出的我不知道意图
5、null == undefined//true undefined继承于null。但是用"绝对等"返回的就是false 这具体的原因网上很多了
6、null >= undefined//??
7、null <= undefined//?
8、parseInt("123abc")//NaN
9、"123abc" - 0 //?
10、Infinity > 10//
11、Infinity > "abc"//
12、Infinity == NaN//
13、true == 1//true 整数比的话 会有内置的转换
14、new String("abc") == "abc"//true
15、new String("abc") === "abc"//false
说出它们的输出结果
1、
var a = "123abc";
alert(typeof(a ));//string
alert(a);//123abc
2、
var a = "123abc";
a.valueOf = function()\{return parseInt(a);\}
//alert( a);//123abc 实质上是a.toString()
//alert(a-0);//NaN
3、
var a = new Object();
a.toString = function()\{return "123abc";\}
a.valueOf = function()\{return parseInt(a);\}
alert( a);//123abc
alert(a-0);//123???为啥


//----------------------zj
var a = new Object();
a.toString = function()\{return "123abc";\}
//a.valueOf = function()\{return parseInt(a);\}
alert( a);//123abc
alert(a-0);//NaN
4、
String.prototype.valueOf = function()
\{
return parseFloat(this);
\}
alert("123abc" > 122);//false
alert(new String("123abc") > 122);//true
//----------------valueOf到底是什么
5、
var s = new String("abc");// s的类型是object
alert(typeof(s) == typeof("abc"));//false
alert(s === "abc");//false
alert(s.toString() == s);//false s是对象 不是隐含调用toString方法么
6、//??????????? toString和toValue的区别呢？
var a = new Object();
a.toString = function()\{return "a"\};
var b = new Object();
b.toString = function()\{return "b"\};
alert(a>b);
a.valueOf = function()\{return 1\};
b.valueOf = function()\{return 0\};
alert(a>b);
7、
function step(n)
\{
return function(x)
\{
return x\*n ;
\}
\}
var a = step(10);//100
var b = step(20);//200
alert(a(10));


alert(b(10));//先将20赋值给n，b返回的是个匿名函数，b再将10传给x 所以10\*20=200 不知道这么解释对不对。a的同理

\-----------------------------------------------割----------------------------------------

今天将部分做出，并简单解释了其原因，另上述部分因个人能力问题，尚未能解释出来。欢迎javascript爱好者帮忙一同学习！ 我也会陆续明后两天将其题目全部答案和原因尽量更新全部。 部分可能解释有误，欢迎指正，部分尚未解释出来的，我会抽空解决！