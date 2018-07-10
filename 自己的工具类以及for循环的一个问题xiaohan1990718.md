---
title: 自己的工具类以及for循环的一个问题
date: 2012-10-26 22:31:58
categories: "开发"
tags:
	- 技术
	- javascript

---

先看例子：

var obj = \{
id:'123',
name:'Hank',
age:22
\};
var array = \['id','name','age'\];
for(var i in obj)\{
console.log(i);//i的值依次为:id name age
\}


for(var j in array)\{
console.log(j);//j的值依次为0 1 2


\}

![IRYQ-YERE-MFME.jpg][]


忘记哪篇博文细讲for in 对象与数组的区别了（昨天的博文或则是昨天看别人的博文）。结论是 for in使用与对象。数组的循环是这样滴~：

for(var i=0,len = array.length;i<len;i++)\{

console.log(array\[i\]);

\}

至于为何不写作：

for(var i=0;i<array.length;i++)\}\{

//your code...

\}

原因：前一代码段长度计算在循环时只计算一次，而后则每次循环都要计算一次。这样就节省计算时间啊~~~~

之上不是重点，今日多写一篇的原因是同事感慨之前没好好的保存自己的代码，特别是可做工具的方法代码（没维护自己的工具类），感觉好可惜。深受其感染，将一些感想的代码和语言好好处理。以下是部分工具类方法：（Javascript版）

/\*\*
\* @FileDirections: 工具类方法--用于调用 简洁代码使用
\* @Author: hank.
\* @E-mail: haw\_king@foxmail.com
\* @Tools: Created with JetBrains WebStorm.
\* @Date: 12-10-19
\* @Time: 下午4:25
\* To change this template use File | Settings | File Templates.
\*/
Tools = \{
/\*\*
\* 循环遍历方法
\* @param arrays 循环对象数组
\* @param callbackFun 回调函数
\*/
forEach:function(arrays,callbackFun)\{
for(var i= 0,len = arrays.length;i<len;i++)\{
callbackFun(arrays\[i\]);
\}
\},
/\*\*
\* for in 循环 使用与对象的遍历
\* @param obj 对象
\* @param callbackFn 回调函数
\*/
forIn:function(obj,callbackFn)\{
for(var i in obj)\{
callbackFn(obj\[i\],i);//i为对象obj的属性 obj\[i\]为对应的属性值
\}
\},
extend:function(childFn,parentFn)\{
var c = childFn.prototype;
var p = parentFn.prototype;
for(var i in p)\{
c\[i\] = p\[i\];
\}
\}
\};


\-----------------------------------------------------------------------------------------

看书，睡觉，坚持写代码！


[IRYQ-YERE-MFME.jpg]: /pro/os/crawler/IRYQ-YERE-MFME.jpg