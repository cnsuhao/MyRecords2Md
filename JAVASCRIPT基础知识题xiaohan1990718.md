---
title: JAVASCRIPT基础知识题
date: 2012-11-03 16:40:56
categories: "开发"
tags:
	- JAVA

---

1、typeof(NaN) 、typeof(Infinity)、typeof(null)、typeof(undefined)
2、NaN == NaN
3、NaN != NaN
4、NaN >= NaN
5、null == undefined
6、null >= undefined
7、null <= undefined
8、parseInt("123abc")
9、"123abc" - 0
10、Infinity > 10
11、Infinity > "abc"
12、Infinity == NaN
13、true == 1
14、new String("abc") == "abc"
15、new String("abc") === "abc"

说出它们的输出结果

1、
var a = "123abc";
alert(typeof(a ));
alert(a);

2、
var a = "123abc";
a.valueOf = function()\{return parseInt(a);\}
alert( a);
alert(a-0);

3、
var a = new Object();
a.toString = function()\{return "123abc";\}
a.valueOf = function()\{return parseInt(a);\}
alert( a);
alert(a-0);

4、
String.prototype.valueOf = function()
\{
return parseFloat(this);
\}
alert("123abc" > 122);
alert(new String("123abc") > 122);

5、
var s = new String("abc");
alert(typeof(s) == typeof("abc"));
alert(s === "abc");
alert(s.toString() == s);

6、
var a = new Object();
a.toString = function()\{return "a"\};
var b = new Object();
b.toString = function()\{return "b"\};
alert(a>b);
a.valueOf = function()\{return 1\};
b.valueOf = function()\{return 0\};
alert(a>b);

7、
function step(a)
\{
return function(x)
\{
return x a ;
\}
\}
var a = step(10);
var b = step(20);
alert(a(10));
alert(b(10));


第一题
var i=5;
function fo()
\{
var i=4;
var fi=new Function("alert(i);");
fi();
\}
fo();

第二题
function f1()
\{
i=20;
eval("var i");
\}
alert(i);
function f1()
\{
i=10;
var i;
\}
alert(i);

第三题
function f1()
\{
alert("1");
\}
function f2()
\{
alert("2");
\}
var f3=f1.call;
f3.call(f2);

第四题
function fo()
\{
var i=0;
return function(n)
\{
return n i ;
\}
\}
fo()(15);
alert(fo()(20));

第五题
function f1(n)
\{
if(n==0)return;
else return f2(--n);
\}
function f2(n)
\{
alert(f1.caller);
return f1(n);
\}
f1(2);

第六题
alert(typeof Function.prototype);
alert(Object.prototype instanceof Function);
alert(Function.prototype instanceof Function);

原型和对象

第一题
function cls1()
\{
this.a=new Array;
\}
function cls2()
\{
\}
cls2.prototype=new cls1;
var x=new cls2;
var y=new cls2;
x.a.push(1,2,3);
alert(y.a);

第二题
function cls1()
\{
this.a=1;
\}
function cls2()
\{
\}
var x=new cls2;
cls2.prototype=new cls1;
var y=new cls2;
alert(x.a);
alert(y.a);

第三题
function cls1()
\{
this.a=1;
\}
function cls2()
\{
\}
var x=new cls2;
cls2.prototype.a=1;
var y=new cls2;
alert(x.a);
alert(y.a);

第四题
function cls1()
\{
this.a=1;
\}
function cls2()
\{
\}
var x=new cls2;
cls2.prototype.a=1;
var y=new cls2;
alert(x.a);
alert(y.a);

第五题
String.prototype.self=function()
\{
return this;
\}
var s="s";
alert("s".self()=="s")
alert(s.self()==s)
alert("s"==="s")
alert("s".self()=="s".self())
alert(s.self()==s.self())


进入名企的javascript面试题
基础部分
1 以下问题简短作答
1.1 Jscript的两种变量范围有什么不同？
1.2 列举Jscript的三种主要数据类型、两种复合数据类型和两种特殊数据类型。
1.3 程序中捕获异常的方法。
2 声明一个字符串数组并初始化，存放用于金额大写的十个中文字符

3 写出下列例程运行的结果
3.1 程序运行完毕后，k等于几？
1
2
3
for (i = 0, j = 0; i < 10, j<6; i , j ) \{
k = i j;
\}

3.2 写出函数DateDemo的返回结果，系统时间假定为今天

function DateDemo() \{

var d, s = "今天日期是: ";

d = new Date();

s = d.getMonth() "/";

s = d.getDate() "/";

s = d.getYear();

return(s);

\}

3.3 写出程序最后一条语句执行后变量result的值


var epsilon = 0.00000000001; // 一些需要测试的极小数字。

function integerCheck(a, b, c)

\{

if ( (a\*a) == ((b\*b) (c\*c)) )

return true;

return false;

\}

function floatCheck(a, b, c)

\{

var delta = ((a\*a) - ((b\*b) (c\*c)))

delta = Math.abs(delta);

if (delta < epsilon)

return true;

return false;

\}

function checkTriplet(a, b, c)

\{

var d = 0;

if (b > a)

\{

d = a;

a = b;

b = d;

\}

if (c > a)

\{

d = a;

a = c;

c = d;

\}

if (((a % 1) == 0) && ((b % 1) == 0) && ((c % 1) == 0))

\{

return integerCheck(a, b, c);

\}

else

\{

return floatCheck(a, b, c);

\}

\}

// 下面的三个语句赋给范例值，用于测试。


var sideA = 5;

var sideB = 5;

var sideC = Math.sqrt(50.001);

var result = checkTriplet(sideA, sideB, sideC);

4 写一个函数，返回指定的英文句子中的每个单词及其字符的起止位置
例：”The rain in Spain falls mainly in the plain.”
应依次返回”The 0-3”, ”rain 4-8” … … ”plain 38-43”
高级部分
5 浏览器对 JScript脚本的解释顺序？
6 判断下列表达式的真假

“100″ == 100;
false == 0;
“100″ === 100;
false === 0;
7 如何为语句设定默认对象（通常用来缩短特定情形下必须写的代码量，使代码变得更短且更易读）？在下面的例子中，请注意 Math的重复使用：
1
2
3
x = Math.cos(3 \* Math.PI) Math.sin(Math.LN10)

y = Math.tan(14 \* Math.E)

8 在对象的属性的个数未知的情况下，如何对该对象的属性进行遍历？
9 书写一个匹配HTML标记的正则表达式
10 构造一个自定义对象，实现对一个矩形的对象化，要求：


a) 描述矩形的标识(name)
b) 描述矩形的颜色(color)
c) 描述矩形的宽度(width)
d) 描述矩形的高度(height)
e) 提供获取矩形面积的方法(getArea())
f) 写出构造函数的完整代码
g) 给出调用的实例代码


javascript面试题

javascript弹出确认框的函数
.text框输入完成后，按回车键跳到下一个text框
.服务器端删除提示的消息框如何写

form中的input有哪些类型？各是做什么处理使用的？
text radio checkbox file button image submit reset hidden


2、table标签中border,cellpadding td标签中colspan,rowspan分别起什么作用？
border边界
cellpadding边距
colspan跨列数
rowspan跨行数


3、form中的input可以设置readonly和disable，请问这两项属性有什么区别？
readonly不可编辑,但可以选择和复制
disable不能编辑复制选择


4、JS中的三种弹出式消息提醒(警告窗口、确认窗口、信息输入窗口)的命令是什么？
alert
confirm
prompt

以下哪条语句会产生运行错误：（）
A.var obj = ();
B.var obj = \[\];
C.var obj = \{\};
D.var obj = //;
2、以下哪个单词不属于javascript保留字：（）
A.with
B.parent
C.class
D.void
3、请选择结果为真的表达式：（）
A.null instanceof Object
B.null === undefined
C.null == undefined
D.NaN == NaN
二、不定项选择题
4、请选择对javascript理解有误的：()
A.JScript是javascript的简称
B.javascript是网景公司开发的一种Java脚本语言，其目的是为了简化Java的开发难度
C.FireFox和IE存在大量兼容性问题的主要原因在于他们对javascript的支持不同上
D.AJAX技术一定要使用javascript技术
5、foo对象有att属性，那么获取att属性的值，以下哪些做法是可以的：（）
A.foo.att
B.foo(“att”)
C.foo\[“att”\]
D.foo\{“att”\}
E.foo\[“a” ”t” ”t”\]
6、在不指定特殊属性的情况下，哪几种HTML标签可以手动输入文本：（）
A.<TEXTAREA></TEXTAREA>
B.<INPUT type=”text”/>
C.<INPUT type=”hidden”/>
D.<DIV></DIV>
7、以下哪些是javascript的全局函数：（）
A.escape
B.parseFloat
C.eval
D.setTimeout
E.alert
8、关于IFrame表述正确的有：()
A.通过IFrame，网页可以嵌入其他网页内容，并可以动态更改
B.在相同域名下，内嵌的IFrame可以获取外层网页的对象
C.在相同域名下，外层网页脚本可以获取IFrame网页内的对象
D.可以通过脚本调整IFrame的大小
9、关于表格表述正确的有：（）
A.表格中可以包含TBODY元素
B.表格中可以包含CAPTION元素
C.表格中可以包含多个TBODY元素
D.表格中可以包含COLGROUP元素
E.表格中可以包含COL元素
10、关于IE的window对象表述正确的有：（）
A.window.opener属性本身就是指向window对象
B.window.reload()方法可以用来刷新当前页面
C.window.location=”a.html”和window.location.href=”a.html”的作用都是把当前页面替换成a.html页面
D.定义了全局变量g；可以用window.g的方式来存取该变量
三、问答题：
1、谈谈javascript数组排序方法sort()的使用，重点介绍sort()参数的使用及其内部机制
2、简述DIV元素和SPAN元素的区别。
3、结合<span id=”outer”><span id=”inner”>text</span></span>这段结构，谈谈innerHTML outerHTML innerText之间的区别。
4、说几条XHTML规范的内容（至少3条）
5、对Web标准化（或网站重构）知道哪些相关的知识，简述几条你知道的Web标准？
四、程序题：
1、完成foo()函数的内容，要求能够弹出对话框提示当前选中的是第几个单选框。
<html>
<body>
<script>
function foo() \{
// 在此处添加代码
return false;
\}
</script>
<body>
<form name="form1" >
<input type="radio" name="radioGroup"/>
<input type="radio" name="radioGroup"/>
<input type="radio" name="radioGroup"/>
<input type="radio" name="radioGroup"/>
<input type="radio" name="radioGroup"/>
<input type="radio" name="radioGroup"/>
<input type="submit"/>
</form>
</body>
</html>
2、填充注释部分的函数体，使得foo()函数调用弹出”成功”的对话框。代码应尽量简短。
<html>
<body>
<script>
function foo() \{
var str = reverse('a,b,c,d,e,f,g');
alert(str);
if (str == 'g,f,e,d,c,b,a') alert('成功');
else alert('失败');
\}
function reverse(str) \{
// 在此处加入代码，完成字符串翻转功能
\}


<script type="text/javascript">
var x = 1;
var y = 0;
var z = 0;
function add(n)\{n=n 1;\}
y = add(x);
function add(n)\{n=n 3;\}
z = add(x);
s=y z;
</script>

求：
y的值是？
z 的值是？
s的值是？

我相信，肯定有同学会答错，当然，不是说他们不会，而是他们可能太大意了！

我们首先看function add，两个add都没有返回值，而我们知道，没有明确返回值的，全部返回undefined，所以，y和z都会是undefined，那么s自然也就不会是一个数字了，没错，s应该是NaN。

假如我们将题目改一下呢？如下：
<script type="text/javascript">
var x = 1;
var y = 0;
var z = 0;
function add(n)\{return n=n 1;\}
y = add(x);
function add(n)\{return n=n 3;\}
z = add(x);
s=y z;
</script>

两个function add都有返回值了，那么，y,z,s会是多少呢？
不错，y和z都是4，s是8，为什么y不是2而是4呢？因为在javascript中，直接通过function申明的函数，后面定义的，会影响到之前的引用，如下：

<script type="text/javascript">
function x()\{alert(2)\};
x();//output 3
function x()\{alert(3)\};
x();//output 3
</script>

如果是通过var来申明的函数会是什么情况呢？我们看看：

<script type="text/javascript">
var x = function()\{alert(0)\};
x();//output 0
var x=function()\{alert(1)\};
x();//output 1
x();//output 1
</script>

通过var申明的函数，后面定义的并不会影响前面的引用。

如果两种模式混合，又会是什么情况呢？

<script type="text/javascript">
function x()\{alert(2)\};
x();//output 3
var x = function()\{alert(0)\};
x();//output 0
var x=function()\{alert(1)\};
x();//output 1
function x()\{alert(3)\};
x();//output 1
</script>

结果是这样的，你猜到了吗？

通过function定义的函数，后面定义的，照旧影响了前面的引用，但是不能改变通过var申明函数后的引用，反而，通过var申明的函数，改变了后来通过function申明函数之后的引用。

所以，如果：
<script type="text/javascript">
var x=function()\{alert(1)\};
x();
function x()\{alert(3)\};
x();
</script>
后面的x()出来的也会是1。

\--------------------------------摘自：http://mianshiti.diandian.com/post/2011-09-05/4728073

有些问题我个人感觉 通过一定的训练会理解的更透彻点。故而转之。