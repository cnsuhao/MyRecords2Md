---
title: 用JavaScript实现一个双向数据绑定，你也可以
date: 2018-04-27 07:12:28
categories: "开发"
tags:
	- 技术
	- JavaScript
	- 编程语言

---

> 点击上方蓝字关注“小郑搞码事”，每天都能学到知识，搞懂一个问题！

我接触双向数据绑定的概念是从两年前刚开始使用angular框架开发项目时候，其中angular框架的特点之一就是双向数据绑定。之后接触react框架，其特点就变成了单向数据绑定，这个大家用过的，一定都非常熟悉。有点绕了......

先简单说一下什么是双向数据绑定，就是用户更新view时，Model的数据也会自动更新，而JavaScript代码更新Model时，View就会自动更新，这就是双向数据绑定。

今天我们用JavaScript来实现一个双向数据绑定。实现的最终效果如下所示：

![用JavaScript实现一个双向数据绑定，你也可以][JavaScript]

从原理，实现，应用场景来讲一下。

# **一、从原理出发** #

我们知道，在JavaScript中，给一个对象添加属性可以像下面这样：

> obj.age = '20'
> 
> obj.say = function \{\}

还可以通过Object.defineProperty来定义新属性或修改原有的属性。其语法如下：

> Object.defineProperty(obj, prop, descriptor)

语法中，obj是需要操作的目标对象，prop是需要定义或修改的属性名字，descriptor所拥有的特性。

然而，关于descriptor目前有两种形式：数据描述和存取器描述。关于存取器描述，它允许我们定义get，set等其它属性。具体如何应用，我给大家举一个例子：

![用JavaScript实现一个双向数据绑定，你也可以][JavaScript 1]

上面这段代码打印出来的是分别是：hello change value

简单说一下上面这段代码，当获取值的时候触发get函数，当设置值的时候触发set函数。设置的新的值由参数value拿到。

通过这个例子，我们知道了如何通过Object.defineProperty来定义一个新属性。下面我们来看一下本篇的重点内容，双向数据绑定的代码如何实现。

# **二、看实现代码** #

![用JavaScript实现一个双向数据绑定，你也可以][JavaScript 2]

上面这是显示的dom的结构。接着来看一实现的JavaScript代码：

![用JavaScript实现一个双向数据绑定，你也可以][JavaScript 3]

通过上面原理的分析，这段代码看着是不是很好理解了。主要就是用了一个keyup的监听事件。

# **三、聊应用场景** #

用双向数据绑定的最常见的应用可以就是表单了。应用场景还是很有限的。

**最后总结一下：**

我觉得关于Object.defineProperty大家可以在去详细了解一下，其次就是关于双向数据绑定的JavaScript实现知道怎么实现及其应用场景。


[JavaScript]: /pro/os/crawler/U3EF-UNYB-BJ22.gif
[JavaScript 1]: /pro/os/crawler/ZF77-JAYE-ZUZM.jpg
[JavaScript 2]: /pro/os/crawler/ERNJ-6BEE-RY2Y.jpg
[JavaScript 3]: /pro/os/crawler/UIQM-ARBJ-BNJR.jpg
 *  **原文作者：** 小郑搞码事
 *  **原文链接：** https://www.toutiao.com/item/6548898431761383943/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。