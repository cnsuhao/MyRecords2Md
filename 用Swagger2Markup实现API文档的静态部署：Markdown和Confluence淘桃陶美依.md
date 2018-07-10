---
title: 用Swagger2Markup实现API文档的静态部署：Markdown和Confluence
date: 2018-01-30 11:48:04
categories: "开发"
tags:
	- Java
	- GitHub
	- Hexo
	- 编程语言
	- Markdown

---

# Swagger2Markup简介 #

Swagger2Markup是Github上的一个开源项目。该项目主要用来将Swagger自动生成的文档转换成几种流行的格式以便于静态部署和使用，比如：AsciiDoc、Markdown、Confluence。

项目主页：https://github.com/Swagger2Markup/swagger2markup

# 如何使用 #

要生成Markdown和Confluence的方式非常简单，与上一篇中的方法类似，只需要修改一个参数即可。

**生成Markdown和Confluence**

生成方式有一下两种：

通过Java代码来生成：只需要修改 withMarkupLanguage属性来指定不同的格式以及 toFolder属性为结果指定不同的输出目录。

生成markdown的代码片段：

![用Swagger2Markup实现API文档的静态部署：Markdown和Confluence][Swagger2Markup_API_Markdown_Confluence]

Java

生成confluence的代码片段：

![用Swagger2Markup实现API文档的静态部署：Markdown和Confluence][Swagger2Markup_API_Markdown_Confluence 1]

Java

在执行了上面的设置内容之后，我们就能在当前项目的src目录下获得如下内容：

![用Swagger2Markup实现API文档的静态部署：Markdown和Confluence][Swagger2Markup_API_Markdown_Confluence 2]

Java

可以看到，运行之后分别在markdown和confluence目录下输出了不同格式的转换内容。

**输出到单个文件**

如果不想分割结果文件，也可以通过替换

toFolder(Paths.get("src/docs/asciidoc/generated")

为

toFile(Paths.get("src/docs/asciidoc/generated/all"))

，将转换结果输出到一个单一的文件中，这样可以最终生成html的也是单一的。

**通过插件输出方式类似，这里不做赘述，如何引入插件可以查看上一篇文章**

# 静态部署 #

下面来看看Markdown和Confluence生成结果的使用。

# Markdown的部署 #

Markdown目前在文档编写中使用非常常见，所以可用的静态部署工具也非常多，比如：Hexo、Jekyll等都可以轻松地实现静态化部署。具体使用方法，这里按照这些工具的文档都非常详细，这里就不具体介绍了。

# Confluence的部署 #

相信很多团队都使用Confluence作为文档管理系统，所以下面具体说说Confluence格式生成结果的使用。

第一步：在Confluence的新建页面的工具栏���选择

\{\}Markup

![用Swagger2Markup实现API文档的静态部署：Markdown和Confluence][Swagger2Markup_API_Markdown_Confluence 3]

Java

第二步：在弹出框的

Insert

选项中选择

ConfluenceWiki

，然后将生成的txt文件中的内容，黏贴在左侧的输入框中；此时，在右侧的阅览框可以看到如下图的效果了。

![用Swagger2Markup实现API文档的静态部署：Markdown和Confluence][Swagger2Markup_API_Markdown_Confluence 4]

Java

注意：所��

Insert

选项中也提供了Markdown格式，我们也可以用上面生成的Markdown结果来使用，但是效果并不好，所以在Confluence中使用专门的生成结果为佳。

Java学习资料获取（复制下段连接至浏览器即可）

data:text/html;charset=UTF-8;base64,5p625p6E5biI5a2m5Lmg6LWE5paZ5YWN6LS56aKG5Y+W6K+35Yqg5omj5omj5Y+35pivMTAxODkyNTc4MA==


[Swagger2Markup_API_Markdown_Confluence]: /pro/os/crawler/YVJI-7JYI-VR3Q.jpg
[Swagger2Markup_API_Markdown_Confluence 1]: /pro/os/crawler/FQA6-N2VB-AMAF.jpg
[Swagger2Markup_API_Markdown_Confluence 2]: /pro/os/crawler/ZIRI-2YRA-6FAZ.jpg
[Swagger2Markup_API_Markdown_Confluence 3]: /pro/os/crawler/UNIV-JBYA-ZUJZ.jpg
[Swagger2Markup_API_Markdown_Confluence 4]: /pro/os/crawler/BZVY-JQZR-BIFI.jpg
 *  **原文作者：** 淘桃陶美依
 *  **原文链接：** https://www.toutiao.com/item/6516685518204305928/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。���载请注明出处。