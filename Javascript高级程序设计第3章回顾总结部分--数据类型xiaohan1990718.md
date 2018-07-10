---
title: Javascript高级程序设计第3章回顾总结部分--数据类型
date: 2012-10-16 22:18:52
categories: "开发"
tags:
	- Java

---

ECMAScript中有5种简单数据类型（基本数据类型），分别为：Undefined.Null.Boolean.Number.String，以及1种复杂数据类型-Object.

Object本质上由一组无序的名值（key:value）对组成的。

\--------------------------------------------------------------------------

1.typeof操作符

typeof是检测ECMAScript的数据类型，因为ECMAScript是松散类型的，所有变量的都使用var声明。

typeof varParams 的返回结果只能会是以下几种字符串之一：

“undefined”:表示varParams未定义。

“boolean”:表示为Boolean类型数据

“number”：Number类型数据

“string”：String类型数据

“object”：对象或者null

“function”:这个值是函数

咋一看是容易与数据类型混淆的，但Javascript是区分大小写的，所以含义是不同的。

2.Undefined

该类型唯一的值时undefined。在使用var声明变量并未初始化时，该变量默认值为undefined。

注意的是：

如果是尚未定义的变量，运行时会报错，但是该变量typeof的值也是undefined。

例子：

var age;//定义但未初始化

alert(age);//"undefined"

alert(name);//ReferenceError: Can't find variable:name

//console.log(name);//为空字符 这点不解。---刚测了下明白了 这应该是http://labs.codecademy.com/\#:workspace的一个问题。name或许是该控制台一个默认全局变量。。。。。。有空再求解。睡。。。。去。

console.log(typeof age);//"undefined"

console.log(typeof name);//"undefined"

3.Null

alert(null==undefined);//true

alert(null===undefined);//false

本书的解释是：undefined值是派生自null值的，ECMAScript-262规定对它们的相等测试要返回true.

但是它们的用途不同。所以“绝对等”测试时，返回的是false.

4.Number

话说本书说要注意的类型，但勾起我注意的是NaN.

NaN是非数值。（Not a Number） ，是一个特殊数值。它有两个特点：任何涉及NaN的操作都会返回NaN。其次，NaN与任何值（注意：任何值是包含NaN自身的）都不相等。

alert(NaN==NaN);//false.

不过好在ECMAScript定义了isNaN()函数。任何不能被转换为数值的值都会导致这个函数返回true.即是数值的话返回false。------这跟我大脑老打架，这个函数字面上定义指的是“是否非数值”--返回true则表示是非数值 即 不是数值， 返回false表示不是非数值 即 是数值。。。。。。。差点乱倒。

\---------------------------------------------------------------------------------------------------------------------------------------------------------------

//明日继续其他类型和第三章其余。累了~~睡觉。以上纯手敲，错误难免。![微笑][QJVY-NZEN-YNZ2.gif]

var a = undefined;
console.log(a===null);//undefined==null为true，但绝对等时为false
var b;
console.log(b==null);//未初始化但var声明的变量默认为undefined
console.log(b===null);


//--------------------
var age;//定义但未初始化
console.log(age);//"undefined"


alert(name1);//ReferenceError: Can't find variable: name1
console.log("----"+name2);//ReferenceError: Can't find variable: name2
console.log(typeof age);//"undefined"
console.log(typeof name3);//"undefined"


console.log(NaN == NaN);//false
console.log(isNaN(2));//false



符测试图：

![RNRJ-IR2A-YVMV.jpg][]


本身给的解释是：


[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif
[RNRJ-IR2A-YVMV.jpg]: /pro/os/crawler/RNRJ-IR2A-YVMV.jpg