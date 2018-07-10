---
title: 理解Javascript_03_javascript全局观
date: 2012-10-18 21:25:29
categories: "开发"
tags:
	- Java
	- java

---

今天让我们站在语言的高度来看一下Javascript都有点什么。因为是全局性的俯瞰，所以不针对细节作详细的讲解。

先来看一张图吧：

![FQNQ-MRFB-NN3M.jpg][]

解释一下：

核心（ECMAScript)：定义了脚本语言的所有对象，属性和方法

文档对象模型（DOM)：HTML和XML应用程序接口

浏览器对象模型(BOM)：对浏览器窗口进行访问操作

现在来具体的讲一个各个成分：

**关于ECMAScript**

ECMAScript的工作是定义语法和对象，从最基本的数据类型、条件语句、关键字、保留字到异常处理和对象定义都是它的范畴。

在ECMAScript范畴内定义的对象也叫做原生对象。

其实上它就是一套定义了语法规则的接口，然后由不同的浏览器对其进行实现,最后我们输写遵守语法规则的程序，完成应用开发需求。

**关于DOM**

根据DOM的定义(HTML和XML应用程序接口)可知DOM由两个部分组成，针对于XML的DOM即DOM Core和针对HTML的DOM HTML。

那DOM Core 和DOM HTML有什么区别与联系呢?

DOM Core的核心概念就是节点(Node)。DOM会将文档中不同类型的元素(这里不元素并不特指<div>这种tag,还包括属性，注释，文本之类）都看作为不同的节点。

![BMRF-7NEM-2QVB.jpg][]

节点结构图

上图描述了DOM CORE的结构图，比较专业,来看一个简单的：

[?][Link 1]

    | ------- | ------------------------------------------------------------------------ |
    | 1  2  3 | `<div id=` `"container"` `>`  `` `<span>hello world</span>`  `` `</div>` |

来看一下这段代码在标准浏览器里的DOM表现:

![MYAI-FYJQ-AINZ.jpg][]

div和span元素被展现成了一个元素节点，对应到节点结构图中的Element元素

"hello world"和div与span之间的间隔，被展现成了文本节点，对应到节点结构图中的CharacterDate元素

DOM CORE在解析文档时，会将所有的元素、属性、文本、注释等等视为一个节点对象（或继承自节点对象的对象,多态、向上转型）,根据文本结构依次展现，最后行成了一棵"DOM树"

DOM HTML的核心概念是HTMLElement，DOM HTML会将文档中的元素（这里的元素特指<body>这种tag,不包括注释，属性，文本)都视为HTMLElement。而元素的属性，则为HTMLElement的属性。

再来看一个示例：

从Node接口提供的属性

myElement.attributes\["id"\].value;很明显myElement.attributes\["id"\]返回一个对象.value是得到对象的value属性

Element实现的方法返回

myElement.getAttributes("id");很明显此时id现在只是一个属性而已，这只是一个得到属性的操作。

其实上DOM Core和DOM html的外部调用接口相差并不是很大，对于html文档可以用DOM html进行操作，针对xhtml可以用DOM Core。

**关于BOM**

老规则，先来一张图：

![RR3I-RIMA-FBA2.jpg][]

BOM与浏览器紧密结合,这些对象也被称为是宿主对象，即由环境提供的对象。

这里要强调一个奇怪的对象Global对象，它代表一个全局对象，Javascript是不允许存在独立的函数，变量和常量，如果没有额外的定义，他们都作为Global对象的属性或方法来看待.像parseInt(),isNaN(),isFinite()等等都作为Global对象的方法来看待，像Nan,Infinity等"常量"也是Global对象的属性。像Boolean,String,Number，RegExp等内置的全局对象的构造函数也是Global对象的属性.但是Global对象实际上并不存在，也就是说你用Global.NaN访问NaN将会报错。实际上它是由window来充当这个角色，并且这个过程是在javascript首次加载时进行的。

\----------------------------------------------------------

摘自：[笨蛋的座右铭][Link 2]：http://www.cnblogs.com/fool/archive/2010/10/08/1846078.html

将的确实细致，虽然明白，但俺讲不出来，所以粘贴至此分享。


[FQNQ-MRFB-NN3M.jpg]: /pro/os/crawler/FQNQ-MRFB-NN3M.jpg
[BMRF-7NEM-2QVB.jpg]: /pro/os/crawler/BMRF-7NEM-2QVB.jpg
[Link 1]: http://www.cnblogs.com/fool/archive/2010/10/08/1846078.html#
[MYAI-FYJQ-AINZ.jpg]: /pro/os/crawler/MYAI-FYJQ-AINZ.jpg
[RR3I-RIMA-FBA2.jpg]: /pro/os/crawler/RR3I-RIMA-FBA2.jpg
[Link 2]: http://www.cnblogs.com/fool/