---
title: 标记语言Markdown常用语法介绍
date: 2017-10-30 08:22:04
categories: "开发"
tags:
	- Drupal
	- 文章
	- HTML
	- 标记语言
	- Markdown

---

# 关于Markdown #

用过github或者gitlab的人都知道，项目根目录一般都会有个READ.md文件，详细介绍此项目内容，下面我们先简单介绍下这门语言的来龙去脉，然后再介绍一些基本的语法和使用。

# 来龙去脉和语法特点 #

Markdown 是一种轻量级标记语言，创始人为约翰·格鲁伯（John Gruber）。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档”。这种语言吸收了很多在电子邮件中已有的纯文本标记的特性。

Markdown 的目标是实现「易读易写」。可读性，无论如何，都是最重要的。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式。总之， Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。比如：在文字两旁加上星号，看起来就像\*强调\*。Markdown 的列表看起来，嗯，就是列表。Markdown 的区块引用看起来就真的像是引用一段文字，就像你曾在电子邮件中见过的那样。

Markdown 语法的目标是：成为一种适用于网络的书写语言。Markdown 不是想要取代 HTML，甚至也没有要和它相近，它的语法种类很少，只对应 HTML 标记的一小部分。Markdown 的构想不是要使得 HTML 文档更容易书写。在我看来， HTML 已经很容易写了。Markdown 的理念是，能让文档更容易读、写和随意改。HTML 是一种发布的格式，Markdown 是一种书写的格式。就这样，Markdown 的格式语法只涵盖纯文本可以涵盖的范围。

正是因为Markdown的这些特点，而且功能比纯文本更强，因此有很多人用它写博客。世界上最流行的博客平台WordPress和大型CMS如joomla、drupal都能很好的支持Markdown。

# 编辑软件 #

如果我们要写Markdown代码的话，我们首先需要一个编辑器，例如Mac里的Mou。

下面是Mou的界面，左边是Markdown代码，右边是实时的展示效果，而且可以选择不同的主题色，非常的漂亮！

![标记语言Markdown常用语法介绍][Markdown]

Mou

当然，很多文档管理或技术类网站都支持Markdown在线编辑，例如有道云笔记，简书等。

# 常用语法介绍 #

这里只介绍最常用和最常见的功能。

**（1）标题**

标题使用不同数量的"\#"来标识是什么层级，可以对应于HTML里面的H1-H6，下面是示例代码和效果

![标记语言Markdown常用语法介绍][Markdown 1]

多级标题

“========”风格的也可以，赶不上"\#"的好用

**（2）图片**

我们可以使用下面的语法，添加一个图片

> !\[Alt text\](/path/to/img.jpg)

详细叙述如下：

一个惊叹号 !

接着一个方括号，里面放上图片的替代文字

接着一个普通括号，里面放上图片的网址

下面是一个示例

![标记语言Markdown常用语法介绍][Markdown 2]

图片引用

**（3）强调**

我们可以使用下面的方式给我们的文本添加强调的效果

\*强调\* 或者 \_强调\_ (示例：斜体)

\*\*加重强调\*\* 或者 \_\_加重强调\_\_ (示例：粗体)

\*\*\*特别强调\*\*\* 或者 \_\_\_特别强调\_\_\_ (示例：粗斜体)

下面是一个示例：

![标记语言Markdown常用语法介绍][Markdown 3]

强调（加精或斜体）

**（4）代码**

如果我们想在文章中添加代码，我们有两种方式

第一种方式是使用反引号(esc键下面的按钮)将代码包裹起来

下面是一个示例代码

![标记语言Markdown常用语法介绍][Markdown 4]

代码展示

第二种方式则是使用制表符或者至少4个空格或一个Tab进行缩进的行

下面是一个示例代码

![标记语言Markdown常用语法介绍][Markdown 5]

代码展示

**（5）换行**

如果我们想把一行文本进行换行，我们可以在需要换行的地方输入至少两个空格，然后回车即可，注意，如果不回车，是没有效果的，就像下面这样

![标记语言Markdown常用语法介绍][Markdown 6]

换行

**（6）引用**

如果我们在文章中引用了资料，那么我们可以通过一个右尖括号">"来表示这是一段引用内容。我们可以在开头加一个，也可以在每一行的前面都加一个。我们还可以在引用里面嵌套其他的引用，下面是一个示例：

![标记语言Markdown常用语法介绍][Markdown 7]

引用

**（7）链接**

如果我们文章中加入一个链接，那么我们通过下面的方式添加

\[链接文字\](链接地址)

例子：

> \[Markdown\](http://blog.csdn.net/zhaokaiqiang1992)

![标记语言Markdown常用语法介绍][Markdown 8]

链接

**（8）分割线**

如果我们想用分割线对内容进行分割，我们可以在单独一行里输入3个或以上的短横线、星号或者下划线实现。短横线和星号之间可以输入任意空格。以下每一行都产生一条水平分割线。

![标记语言Markdown常用语法介绍][Markdown 9]

分割线

**（9）列表标记**

如果我们的内容需要进行标记，那么我们可以使用下面的方式

![标记语言Markdown常用语法介绍][Markdown 10]

标记


[Markdown]: /pro/os/crawler/R7JF-V2YZ-ENEF.jpg
[Markdown 1]: /pro/os/crawler/YAN3-YU3M-7VQB.jpg
[Markdown 2]: /pro/os/crawler/AMVQ-UYMA-AZBJ.jpg
[Markdown 3]: /pro/os/crawler/BZ3I-NIVZ-777J.jpg
[Markdown 4]: /pro/os/crawler/7JNN-ZJQR-ZIR3.jpg
[Markdown 5]: /pro/os/crawler/73MN-MYVJ-NBM3.jpg
[Markdown 6]: /pro/os/crawler/J7FM-2QII-IRBI.jpg
[Markdown 7]: /pro/os/crawler/QRMU-2YBF-NAVR.jpg
[Markdown 8]: /pro/os/crawler/IRBU-AJV2-IEEF.jpg
[Markdown 9]: /pro/os/crawler/VJ7F-UMBV-Z3Y3.jpg
[Markdown 10]: /pro/os/crawler/BQMY-MMYU-EJ2A.jpg
 *  **原文作者：** 木纳哥
 *  **原文链接：** https://www.toutiao.com/item/6481465519395308045/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。