---
title: SiteServer CMS 四种图片解决方案
date: 2017-07-04 16:18:53
categories: "开发"
tags:
	- 科技
	- 文章
	- HTML

---

> **如果您觉得文章对您有点用，麻烦在您阅读、收藏、转发的时候，顺手帮忙点个赞、留个言、加关注，这是我继续写下去的绝佳动力。**

本篇主要转载官方网站上的一篇文章，介绍关于SiteServer CMS系统中调用图片的几种方案，感觉总结的挺全面的，在此推荐一下。

# 一、图片模型方案 #

图片模型方案思路自然合理，功能也最全面。对于图片的ImageUrl和Title都可以单独操作，非常灵活。所以如果需要针对比较专业或复杂的图片集进行管理的话，建议使用这种方案。

使用这种方案也特别简单，只需要把栏目内容模型为“图片”，就可以在文章中添加多张图片，每张图片可以添加单独的描述，添加图片时自动生成缩略图。

**1、后台管理**

![SiteServer CMS 四种图片解决方案][SiteServer CMS]

如上图所示，在栏目管理菜单中把栏目内容模型为“图片”即可。设置完成之后在此栏目下进行内容管理时，每次添加内容之后都会出现：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 1]

点击批量添加按钮则可以一次性上传多张图片，上传完之后是这样的效果：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 2]

或者在文章列表中的操作中也能看到图片管理链接（如下图所示），点击也能进到上面批量管理图片界面：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 3]

**2、模板标签调用**

SiteServer CMS系统模板标签语言STL中有一个stl:eacth标签，可以把此内容的所有图片调取出来，具体用法类似：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 4]

模板解析之后生成的Html代码类似这样：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 5]

通过浏览器访问此页面显示效果类似这样：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 6]

# 二、多篇文章方案 #

这种方案简单描述来说就是建一个栏目，底下发多篇内容，每篇内容的标题（Title）和图片（ImageUrl），变通为图片的Url和图片标题。

这种方案在内容管理上和正常的内容发布是一样的，但比第一种图片模型方案更灵活，因为除了图片描述（标题）之外，如果需要，还可以发布图片简介（内容简介字段）等一切内容有的字段都可以用于图片描述，也即每张图片可以带有多个字段来描述。

**1、后台管理**

比如网站首页，经常会需要多张大的Banner图进行轮播，轮播时还需要为每张图配上一段话（Sologan之类）。这种最适合用这种方案来实现了，在后台建一个“首页轮播图”栏目，发多篇内容，如下图所示：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 7]

每篇内容除了发标题和图片之外，还可以发简介（可以根据实际需求来决定是否需要）：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 8]

**2、模板标签调用**

使用SiteServer CMS系统模板标签语言STL中的stl:contents内容标签，可以把内容中的所有图片调取出来，具体用法类似：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 9]

模板解析之后生成的Html代码类似这样：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 10]

通过浏览器访问此页面显示效果类似这样：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 11]

# 三、单文章多图方案 #

这种方案就是在发一篇内容时，同时发布多张图片，然后前台负责把某篇文章的多个图片显示出来。

这种方案相对来说就不是那么的灵活了，因为每一张图片并不能对应有相应的描述了，所以此方案适合于只需要显示多张图片不需要其他信息的需求，最多就是在显示多张图片之前或之后显示一下内容标题和内容简介，以此作为整个图片组的介绍。

**1、后台管理**

![SiteServer CMS 四种图片解决方案][SiteServer CMS 12]

如下图所示，在发一篇文章时，同时上传多张图片。

**2、模板标签调用**

依然使用SiteServer CMS系统模板标签语言STL中的stl:eacth标签，可以把某篇容的多张图片调取出来，具体用法类似：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 13]

模板解析之后生成的Html代码类似这样：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 14]

通过浏览器访问此页面显示效果类似这样：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 15]

# 四、编辑器内传多图方案 #

这种方案就是将多张图片通过编辑器上传，前台调用文章内容的方式把图片调用到前端。

这种方式最简单，在所见即所得的编辑器中添加图片，前台调用即可。但是灵活性不好，基本后台添加成什么样子前台就是什么样子了。比如此种方案就无法直接实现轮播图的效果。因为这是带前台表现样式了，所以无法和页面模板效果配合起来灵活使用。

**1、后台管理**

和正常内容发布一样的操作流程，只是在内容编辑器中同时传了多张图片，可以给图片加描述也可以不加。如下图所示：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 16]

**2、模板标签调用**

使用SiteServer CMS系统模板标签语言STL中的stl:contents内容标签，可以把内容中的所有图片调取出来，具体用法类似：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 17]

最后显示的效果如下图：

![SiteServer CMS 四种图片解决方案][SiteServer CMS 18]

# 开心一笑 #

> 一天晚上3个人去住旅馆，300元一晚。每人掏100元凑够300元交给了老板。
> 
> 后来老板说今天搞活动，优惠50元，命令服务生退还给他们三人。
> 
> 服务生偷偷藏起了20元，把剩下的30元钱分给了他们三个人，每人分到10元。
> 
> 这样刚才每人掏了100元现在又退回10元也就是90元。
> 
> 就是说每人只花了90元钱，3个人每人90元就是270元，3 \* 90 = 270，再加上服务生藏起的20元就是290元，还有10元钱去了哪里？


[SiteServer CMS]: /pro/os/crawler/ZZYQ-6NAM-YJZU.jpg
[SiteServer CMS 1]: /pro/os/crawler/E2QZ-ZUY6-BVIN.jpg
[SiteServer CMS 2]: /pro/os/crawler/UURQ-RBMR-RRB2.jpg
[SiteServer CMS 3]: /pro/os/crawler/NEUV-AAUR-2UMV.jpg
[SiteServer CMS 4]: /pro/os/crawler/YBJ3-AZNY-Y63I.jpg
[SiteServer CMS 5]: /pro/os/crawler/EZNR-IFN3-IJBU.jpg
[SiteServer CMS 6]: /pro/os/crawler/UU7F-3YJN-MJRF.jpg
[SiteServer CMS 7]: /pro/os/crawler/MZNM-JE2I-YQNE.jpg
[SiteServer CMS 8]: /pro/os/crawler/YAFZ-RJVI-N7VF.jpg
[SiteServer CMS 9]: /pro/os/crawler/MAQQ-6ZIM-NEUR.jpg
[SiteServer CMS 10]: /pro/os/crawler/QIJ2-YBIB-EIBR.jpg
[SiteServer CMS 11]: /pro/os/crawler/QFYR-AZAJ-6BBF.jpg
[SiteServer CMS 12]: /pro/os/crawler/JREY-INNY-EQ7V.jpg
[SiteServer CMS 13]: /pro/os/crawler/FYZR-MUFI-IEQZ.jpg
[SiteServer CMS 14]: /pro/os/crawler/M7VM-JFJQ-VN3I.jpg
[SiteServer CMS 15]: /pro/os/crawler/NQZB-ANQE-VY2I.jpg
[SiteServer CMS 16]: /pro/os/crawler/6FNI-UJEZ-ERZY.jpg
[SiteServer CMS 17]: /pro/os/crawler/UNEB-R2NU-AQEZ.jpg
[SiteServer CMS 18]: /pro/os/crawler/EU77-VIVB-JFUZ.jpg
 *  **原文作者：** 深入浅出SiteServer
 *  **原文链接：** https://www.toutiao.com/item/6438825254121898498/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。