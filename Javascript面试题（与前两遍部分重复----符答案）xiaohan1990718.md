---
title: Javascript面试题（与前两遍部分重复----符答案）
date: 2012-11-05 20:04:35
categories: "开发"
tags:
	- Java

---

1.求y和z的值是多少？
<script type="text/javascript">
var x = 1;
var y = 0;
var z = 0;
function add(n)\{n=n+1;\}
y = add(x);
function add(n)\{n=n+3;\}
z = add(x);
</script>
y、z都为undefined，因为没有返回值。另外 var result = y\*z; 那么result的值应该是NaN,其实最应该注意的一点是函数里的一个陷阱：木有返回值！！！！所以是undefined。至于为何函数没有返回值会返回undefined。详见我《[Javascript函数返回值的一个问题(显式返回和非显式返回值的问题][Javascript]）》博文。

2.javascript是面向对象的，怎么体现javascript的继承关系？
用prototype来实现。原型链继承是js实现继承的方法--实现方法见我之前关于javascript继承的博文。

3.javascript怎样选中一个checkbox，怎样设置它无效？
document.all.checkbox\[0\].disabled = true;//即设置了其不能编辑使用

4.form中的input可以设置为readonly和disable，请问2者有什么区别？
readonly不可编辑，但可以选择和复制；值可以传递到后台
disabled不能编辑，不能复制，不能选择；值不可以传递到后台

5.js中的3种弹出式消息提醒（警告窗口，确认窗口，信息输入窗口）的命令式什么？
alert
confirm （附注：它有两个返回值 true/false）
prompt

6.form中的input有哪些类型？

text button password (利用编译器的提示功能，其全部type就都出来了)
7.javaScript的2种变量范围有什么不同？

全局变量：当前页面内有效，页面关闭后该变量被清除（或者自己做处理）

局部变量：方法体内有效，并且方法执行后该变量在栈内清除

8.列举javaScript的3种主要数据类型，2种复合数据类型和2种特殊数据类型。

主要数据类型：string, boolean, number

复合数据类型：function, object

特殊数据类型：undefined,null

9.程序中捕获异常的方法？

window.error

try\{\}catch()\{\}finally\{\}

10.写出函数DateDemo的返回结果，系统时间假定为今天

function DateDemo()\{

var d, s="今天日期是：";

d = new Date();

s += d.getMonth() + "/";

s += d.getDate() + "/";

s += d.getYear();

return s;

\}

结果：今天日期是：这里有个陷阱是d.getYear() 返回的是从格林尼治时间（1900年）后的年数，月份则从0开始，如果有星期的话也是从0开始！！以您天为例是 ：10/5/112



11.写出程序运行的结果？

for(i=0, j=0; i<10, j<6; i++, j++)\{

k = i + j;

\}

结果：10（小心陷阱），j=5时i=5;此时k=10;再循环时j=6,i=6此时已经不满足j<6的条件了，所以循环跳出，因而k=10.

12.运行的结果？

function hi()\{
var a;
alert(a);
\}

结果：undefined//a声明了但未赋值，所以未定义

13.运行的结果？

function hi()\{
var a = null;
alert(a);
\}

结果：null//12、13两题类型函数返回值的问题，详见我《[Javascript函数返回值的一个问题(显式返回和非显式返回值的问题][Javascript]》博文。

14.浏览器的对象模型？

window

顶级对象

window.alert(msg)

window.prompt()

window.confirm()

if(window.confirm())\{

...

\}

window.open()

window.close()

document

document.write()

history

当用户浏览网页时，浏览器保存了一个最近所访问网页的url列表。这个列表就是用history对象表示。

history.back():后退,

history.forward():前进

history.go(n):正数表示向前，负数表示向后

location

表示当前打开的窗口或框架的URL信息。

location.href：重定向

等价于location.assign(url)

location.host：类似www.qq.com:8080

navigator

表示浏览器的信息及js运行的环境

navigator.cookieEnabled：该属性表示是否启用cookie

screen

用于显示网页的显示器的大小和颜色

screen.width/screen.height：表示显示器的分辨率（总的宽度，高度）

\---以上答案可www.w3schol.com.cn学习。

15.XMLHTTPRequest对象是什么？
 Ajax原理~

16.超链接的属性target的可选值：\_blank, \_parent, \_self, \_top和框架名称有什么区别？

\_blank重新打开新的窗口。\_parent则是覆盖上层窗口，\_self是本窗口内，\_top是最顶层的窗口。

17.javascript的常用对象有哪些？

String, Math, Date和Array对象、正则等内置对象。

18.innerHTML，innerText，outerHTML，outerText的区别？

这个不清楚了~按照习惯innerHTML一般我用于页面的输出或者替换页面元素内容，innerText则应用于文本的。innerText建议常用。 其他俩没咋用，就不说了。


[Javascript]: http://blog.csdn.net/xiaohan1990718/article/details/8144749