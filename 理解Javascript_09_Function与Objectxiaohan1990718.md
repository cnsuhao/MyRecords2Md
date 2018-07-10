---
title: 理解Javascript_09_Function与Object
date: 2012-11-12 22:01:23
categories: "开发"
tags:
	- Java

---

**Function**

　　首先回顾一下函数对象的概念，函数就是对象,代表函数的对象就是函数对象。所有的函数对象是被Function这个函数对象构造出来的。也就是说，Function是最顶层的构造器。它构造了系统中所有的对象，包括用户自定义对象，系统内置对象，甚至包括它自已。这也表明Function具有自举性(自已构造自己的能力)。这也间接决定了Function的\[\[call\]\]和\[\[constructor\]\]逻辑相同。

[?][Link 1]

    | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | 1  2  3  4  5  6  7  8 | `function Foo() {};`  `var foo = new Foo();`  `//Foo为foo的构造函数`  `alert(foo instanceof Foo); // true`  `//但是Function并不是foo的构造函数`  `alert(foo instanceof Function); // false`  `//Function为Foo的构造函数`  `alert(Foo instanceof Function);//true` |

上面的代码解释了foo和其构造函数Foo和Foo的构造函数Function的关系。(具体原理请参见Function与Object的内存关系图)

**Object　**

　　对于Object它是最顶层的对象，所有的对象都将继承Object的原型，但是你也要明确的知道Object也是一个函数对象，所以说Object是被Function构造出来的。（关于Object并没有太多的理论）

**Function与Object**

这是本文的重点，非常重要！

[?][Link 1]

    | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | 1  2  3  4  5  6  7  8  9  10  11 | `alert(Function instanceof Function);//true`  `alert(Function instanceof Object);//true`  `alert(Object instanceof Function);//true`  ``  `function Foo() {};`  `var foo = new Foo();`  `alert(foo instanceof Foo); // true`  `alert(foo instanceof Function); // false`  `alert(foo instanceof Object); // true`  `alert(Foo instanceof Function); // true`  `alert(Foo instanceof Object); // true` |

你能理解这些答案吗？那恭喜你，Javascript语言的本质你已经理解了。

那么让我们来看一下Object与Function实际的关系吧：

![QIRJ-AYAY-NQUU.jpg][]

在你看图之前，请先阅读函数对象与instanceof原理两篇文章，要不然内存图很难理解。

在这，我对内存图做一点说明：在函数对象一文中提到了函数对象的构造过程，在本文中提到Function为自举性的，所以说函数对象Foo的构造过程和函数对象Function的构造过程是一样的。所以在图中给于高亮显示，我用'|'来分隔来表示它们的构造过程相同。根据instanceof的理论，和内存图，可以将上面的语句都推导出正确的结果。在此我们不一一讲述了，读者自已体会吧。

如果你不能理解这张复杂的内存图的话，可以看下面的说明图来帮助理解：

![BQRV-6FAQ-JY6V.jpg][]

注：代码的实际执行流程并不完全像这张图上描述的那样，也就是说这张图是有问题的(可以说是错误的),它无法解释为什么Function instanceof Function 为true。 但是它易于理解Function与Object的关系。


[Link 1]: http://www.cnblogs.com/fool/archive/2010/10/15/1851851.html#
[QIRJ-AYAY-NQUU.jpg]: /pro/os/crawler/QIRJ-AYAY-NQUU.jpg
[BQRV-6FAQ-JY6V.jpg]: /pro/os/crawler/BQRV-6FAQ-JY6V.jpg