---
title: Javascript函数返回值的一个问题(显式返回和非显式返回值的问题)
date: 2012-11-03 22:20:45
categories: "开发"
tags:
	- Java

---

先上代码：

``````````
function reback(){return ;};
console.log(reback());//undefined

function reback2(){};
console.log(reback2());//undefined

function reback3(){return null;};
console.log(reback3());//null

function reback4(){var s;return s;};
console.log(reback4());//undefined
``````````

如上述代码：

reback函数有显式返回return关键字，但是没有任何返回参数；而reback4中有显式返回return关键字，也有返回参数，但是该参数s为定义，所以返回的是undefined

而reback3的参数是null因而毫无疑问返回的是null; 但是reback2就有点纠结了，它连return关键字都没有，但为何它返回的也是undefined呢？

刚刚网络差的要命的时候看见张龙的javascript视频发现这个例子，然后就有了上述四个函数举例。他给的是一句结论：在javascript中如果函数没有声明返回值，那么会返回undefined。至于到底是为什么，我暂时还不是很清楚。

所说reback3和reback4 这都好理解。 而reback有return关键字，但是并未调用任何参数，实质上也应给跟reback4相同。

**至于reback2到底为何？**明天我查下再做更新，为防止我忘记此时，更新于此。//纯手敲，错误难免![微笑][QJVY-NZEN-YNZ2.gif] 欢迎讨论。我Q:haw\_king@foxmail.com(主显示号)--天徵之。




[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif