---
title: 用JavaScirpt实现斐波纳契数列最简洁方式
date: 2012-10-12 22:09:53
categories: "开发"
tags:
	- Java

---

斐波纳契[数列][Link 1]（Fibonacci Sequence），又称[黄金分割][Link 2]数列，指的是这样一个数列：1、1、2、3、5、8、13、21、……在[数学][Link 3]上，斐波纳契数列以如下被以递归的方法定义：F0=0，F1=1，Fn=F(n-1)+F(n-2)（n>=2，n∈N\*）在现代物理、准晶体结构、化学等领域，斐波纳契数列都有直接的应用，为此，[美国][Link 4]数学会从1960年代起出版了《斐波纳契数列》季刊，专门刊载这方面的研究成果。

 \---以上信息来自百度百科。

网上看了N多，如用if else 、switch case等方式，用三目运算是最简洁的：

 var fib = function(n)\{


 return n < 2 ? n:(fib(n-1)+fib(n-2));//写更简洁的代码
 \};
 console.log(fib(9));



简洁不错的(网上要求是不运用条件判断语句实现最简洁版本，但无法完成校验)，以上实现疏忽两点：非Number类型、非正整值。

完整实现：（使用了条件判断语句,使用正则校验)

 //采用三目运算解决 fib函数问题
var fib = function(n)\{
//是否是正整数
if(!/^\[0-9\]\*\[1-9\]\[0-9\]\*$/.exec(n)) return null;
return n < 2 ? n:(fib(n-1)+fib(n-2));
\};

console.log(fib('a'));//null;
console.log(fib(0));//null
console.log(fib(6));//8

有其他写法没？求解~~~~~~~~~~~~~~~~~~~~~~~~~~~~


 经：http://labs.codecademy.com/\#:workspace 验证通过![微笑][QJVY-NZEN-YNZ2.gif]

![UBMQ-IBMM-EQAQ.jpg][]


\---------------------------------10月29日更改：

本来想将上述例子更改为：

var fib(n)\{

//..

return n<2?arguments.callee(n-1)+arguments.callee(n-2);

\}

这样做的目的是放置函数名称变更引发的错误。但今天看到以下博文：http://www.cnblogs.com/TomXu/archive/2011/12/29/2290308.html（摘自部分）

将来的ECMAScript-262第5版（目前还是草案）会引入所谓的**严格模式（strict mode）**。开启严格模式的实现会禁用语言中的那些不稳定、不可靠和不安全的特性。据说出于安全方面的考虑，`arguments.callee`属性将在严格模式下被“封杀”。因此，在处于严格模式时，访问```arguments.callee`会导致`TypeError`（参见ECMA-262第5版的10.6节）。而我之所以在此提到严格模式，是因为如果在基于第5版标准的实现中无法使用`arguments.callee`来执行递归操作，那么使用命名函数表达式的可能性就会大大增加。从这个意义上来说，理解命名函数表达式的语义及其bug也就显得更加重要了。



``````````
// 此前，你可能会使用arguments.callee
  (function(x) {
    if (x <= 1) return 1;
    return x * arguments.callee(x - 1);
  })(10);
  
  // 但在严格模式下，有可能就要使用命名函数表达式
  (function factorial(x) {
    if (x <= 1) return 1;
    return x * factorial(x - 1);
  })(10);
  
  // 要么就退一步，使用没有那么灵活的函数声明
  function factorial(x) {
    if (x <= 1) return 1;
    return x * factorial(x - 1);
  }
  factorial(10);
``````````

所以貌似这种写法就~~。

另有关公有私有保护方法命名规范一事，今晚写。 地址是：http://blog.csdn.net/xiaohan1990718/article/details/8120161 在分割线下面。



[Link 1]: http://baike.baidu.com/view/39749.htm
[Link 2]: http://baike.baidu.com/view/1816.htm
[Link 3]: http://baike.baidu.com/view/1284.htm
[Link 4]: http://baike.baidu.com/view/2398.htm
[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif
[UBMQ-IBMM-EQAQ.jpg]: /pro/os/crawler/UBMQ-IBMM-EQAQ.jpg