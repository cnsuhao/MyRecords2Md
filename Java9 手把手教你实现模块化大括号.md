---
title: Java9 手把手教你实现模块化
date: 2017-12-05 11:38:46
categories: "开发"
tags:
	- Java
	- 技术
	- IntelliJ IDEA
	- 编程语言

---

IntelliJ IDEA 2017.1 支持Java9的模块化特性 (Project Jigsaw)。 在此版本，模块文件中还支持基于特定名称与关键字的自动补全功能 code completion， 并且你可以快速斧正你项目中的模块化代码。

So，那就让我们来一探究竟什么是模块化工程。首先，我们创建一个普通的模块工程 IntelliJ IDEA module 其中包含了改变世界的伟大的 “Hello World” 。

![Java9 手把手教你实现模块化][Java9]

A simple application

IntelliJ IDEA 会引导你在工程中为你的模块创建一个module-info.java

![Java9 手把手教你实现模块化][Java9 1]

Create new module-info.java

module-info.java 将为你提供一个基础的模块代码结构。

![Java9 手把手教你实现模块化][Java9 2]

Default module-info.java

一切听指挥，党会指引你正确的道路！

此时，当你在按照以往那样使用一个Java类时，你就会看到模块化带来的新姿势。

![Java9 手把手教你实现模块化][Java9 3]

Error using Logger

Here，IntelliJ IDEA可以帮助你找出问题所在，并提出修复建议。

![Java9 手把手教你实现模块化][Java9 4]

Add requires to module-info.java

如你所料，进行这个操作之后IntelliJ IDEA对module-info.java文件进行了正确的更改。

![Java9 手把手教你实现模块化][Java9 5]

Requires added

当然，你也可以尝试自己编辑module-info.java文件，IDEA 会给你完整的补全和提示信息。

![Java9 手把手教你实现模块化][Java9 6]

Code completion in module-info.java

快速修复不仅可用于标准Java模块，还可帮助您自己编写模块代码。如果您尝试访问另一个IntelliJ IDEA模块中的代码，从一个模块的内部来使用模块化特性 (module-info.java

文件中会有提示), IntelliJ IDEA 将会提示你如果没有进行正确的更改，是不能运行的。

![Java9 手把手教你实现模块化][Java9 7]

Using other modules

首先，有很多提示来帮助你完成更改，所以，一旦module-info.java文件所在的模块下有java文件，你可以快速补齐后面的包路径。

![Java9 手把手教你实现模块化][Java9 8]

Code completion for exports

回到这个被导入模块的类中来，使用Alt 和 Enter 来获取fix建议。这里有两个步骤：firstly, 模块 one需要依赖模块two。secondly，一旦此操作完成，模块one的module-info.java文件就能关联模块two。

![Java9 手把手教你实现模块化][Java9 9]

Quick fixes for using modules

这里有两个模块，需要格外注意的是：firstly, the IntelliJ IDEA modules 你可能已经熟悉; and secondly, 新的 Java 9 (Jigsaw) 模块被指定使用module-info.java

。 要使用Java模块化特性，每个Java 9模块都需要对应于IntelliJ IDEA模块。还要注意的是（IntelliJ IDEA 2017.1中的最后一个示例所示），需要声明IntelliJ IDEA模块依赖关系以及Java 9模块依赖关系。所以在最后一个例子中，模块one依赖模块two:

![Java9 手把手教你实现模块化][Java9 10]

IntelliJ IDEA module dependencies

但是在module-info.java文件中也需要声明它是关联模块two的：

![Java9 手把手教你实现模块化][Java9 11]


[Java9]: /pro/os/crawler/RYAV-7VQZ-QV3E.jpg
[Java9 1]: /pro/os/crawler/MII3-EEBB-RVJE.jpg
[Java9 2]: /pro/os/crawler/6BFM-R3RV-NUJJ.jpg
[Java9 3]: /pro/os/crawler/VA6F-NIR2-QF2I.jpg
[Java9 4]: /pro/os/crawler/AUBM-3YYB-ZENV.jpg
[Java9 5]: /pro/os/crawler/BMMA-ZMFZ-UB7R.jpg
[Java9 6]: /pro/os/crawler/2MJF-RB63-UYVE.jpg
[Java9 7]: /pro/os/crawler/VJFN-RUYB-QR7J.jpg
[Java9 8]: /pro/os/crawler/ZURQ-BJUV-ARA2.gif
[Java9 9]: /pro/os/crawler/ZAZU-FZJY-EM2Q.gif
[Java9 10]: /pro/os/crawler/QFZ6-FRM6-3UNV.jpg
[Java9 11]: /pro/os/crawler/EERR-VUNA-7ZAY.jpg
 *  **原文作者：** 大括号
 *  **原文链接：** https://www.toutiao.com/item/6495902354955567629/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。