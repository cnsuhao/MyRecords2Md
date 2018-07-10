---
title: 需要做依赖自由Ajax？
date: 2017-04-06 00:10:21
categories: "开发"
tags:
	- jQuery
	- JavaScript
	- HTML

---

使用jQuery的很大原因之一就是Ajax的简单化程度。它具有针对所有Ajax方法的超级干净，灵活和跨浏览器兼容的API。jQuery仍然是大受欢迎的，但它变得越来越普遍，因为它越来越常见，特别是随着旧版浏览器的分享和新的浏览器有很多强大的东西，我们用来学习jQuery。即使只是querySelectorAll经常被认为是丢失jQuery依赖的原因。

**Ajax如何做？**

假设我们需要做一个GET请求来从URL端点获取一些HTML。我们不会做任何错误处理来保持这个简短。

jQuery会是这样的：

![需要做依赖自由Ajax？][Ajax]

如果我们想要使用jQuery来浏览本机的Ajax，我们可以这样做：

![需要做依赖自由Ajax？][Ajax 1]

浏览器对此的支持有点复杂。基本工作远远高于IE 7，但是IE 10是真的很稳固。如果你想要更强大，但仍然跳过任何依赖关系，你甚至可以使用window.ActiveXObject回退，并得到IE 6。

长篇小说，它绝对有可能做Ajax没有任何依赖，并得到相当深入的浏览器支持。记住，jQuery只是JavaScript，所以你可以随时做任何事情。

但是jQuery在Ajax已经有一段时间一直在做的另一件事：它是Promise基于的。Promises中许多很酷的东西之一，特别是与“Ajax”类似的“异步”结合在一起，它允许您并行运行多个请求，这些是需要执行的。

我刚刚发布的本机Ajax的东西不是基于Promise的。

如果您想要一个强大而方便的基于Promise的Ajax API，具有相当不错的跨浏览器支持（直到IE 8），您可以考虑Axios。是的，它是一个依赖，就像jQuery一样，它只是超级关注Ajax，在GZip之前是11.8 KB，并没有任何依赖于它自己的。

使用Axios，代码将如下所示：

![需要做依赖自由Ajax？][Ajax 2]

请注意当时的声明，这意味着我们回到了Promise土地。微小的侧面注释，显然请求在服务器端看起来与jQuery看起来不一样。

虽然浏览器还没有完成，有一个相当新的Fetch API，使用基于Promise的Ajax，具有很好和干净的语法：

![需要做依赖自由Ajax？][Ajax 3]

浏览器对此的支持也变得非常好！它在所有稳定的桌面浏览器中运送，包括Edge。危险区域根本没有IE支持！


[Ajax]: /pro/os/crawler/ZAIV-RUQA-IEIN.jpg
[Ajax 1]: /pro/os/crawler/EQUY-MUIA-VYNA.jpg
[Ajax 2]: /pro/os/crawler/FMJB-U2YE-VEBZ.jpg
[Ajax 3]: /pro/os/crawler/ERUA-VVQF-ZAAU.jpg
 *  **原文作者：** Web趣事分享
 *  **原文链接：** https://www.toutiao.com/item/6400284334355382786/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。