---
title: 使用Jekyll 开发个人博客
date: 2017-03-26 20:13:48
categories: "开发"
tags:
	- Linux
	- 文章
	- GitHub
	- 编程语言
	- Markdown

---

# 简介 #

本人介绍如何使用Github Markdown Jekyll 开发个人博客.当然本文所讲内容中github的替代方案还有coding.net(码市),git.oschina.net(码云).  


# GitHub简介 #

Git是一个分布式的版本控制系统，最初由Linus Torvalds编写，用作Linux内核代码的管理。在推出后，Git在其它项目中也取得了很大成功，尤其是在Ruby社区中。目前，包括Rubinius、Merb和Bitcoin在内的很多知名项目都使用了Git。Git同样可以被诸如Capistrano和Vlad the Deployer这样的部署工具所使用。

**以上来自百度百科**

# Markdown简介 #

Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。 Markdown具有一系列衍生版本，用于扩展Markdown的功能（如表格、脚注、内嵌HTML等等），这些功能原初的Markdown尚不具备，它们能让Markdown转换成更多的格式，例如LaTeX，Docbook。Markdown增强版中比较有名的有Markdown Extra、MultiMarkdown、 Maruku等。这些衍生版本要么基于工具，如Pandoc；要么基于网站，如GitHub和Wikipedia，在语法上基本兼容，但在一些语法和渲染效果上有改动。  


**以上来自百度百科**

# Jekyll简介 #

jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务,例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。

# 好处 #

我们可以像提交代码一样来管理我们的博客,简单方便快捷,而且还免费. 在此感谢GitHub!

# 搭建本地开发环境 #

本文开发环境基于Ubuntu Kylin 16.04 LTS

 *  安装Ruby

![使用Jekyll 开发个人博客][Jekyll]

 *  安装gem

这个按照官网的教程安装上就可以了

gem官网:https://rubygems.org/pages/download

 *  使用gem安装jekyll

![使用Jekyll 开发个人博客][Jekyll 1]

# 选择Jekyll themes并clone到本地 #

我选择的是 > Jekyll theme链接:https://github.com/liungkejin/liungkejin.github.io

 *  点击github 的 fork功能 将代码fork到自己的仓库
 *  git clone 刚刚fork的仓库到本地

![使用Jekyll 开发个人博客][Jekyll 2]

 *  创建自己的博客文件夹

![使用Jekyll 开发个人博客][Jekyll 3]

 *  复制\`liungkejin.github.io\`到\`blog\`

![使用Jekyll 开发个人博客][Jekyll 4]

 *  本地调试

![使用Jekyll 开发个人博客][Jekyll 5]

 *  等待运行完成后,在控制台也会打印出URL,复制此URL用浏览器打开即可访问,比如输入localhost:4000. **注意此处可能也会输入localhost:4000/blog 这个要根据\_config.yml文件中是否配置baseurl来决定**

# push本地blog文件夹到github仓库 #

 *  首先需要在github上创建blog仓库
 *  然后在\`/home/barton/develop/code/git/blog\`文件夹下执行一下命令:

![使用Jekyll 开发个人博客][Jekyll 6]

# 在blog仓库中设置开启pages功能 #

网上介绍这个功能的文章有很多,咱就不再重复一便了… 进行到此步骤,在设置中就可以看到github分配给你的域名,比如说:sunshineasbefore.github.io/blog 点击此域名即可访问使用github pages功能搭建的静态博客.

# 博客配置 #

 *  Jekyll的语法以及目录结构请参考Jekyll的中文文档.
 *  更改\_config.yml

![使用Jekyll 开发个人博客][Jekyll 7]

 *  更改作者设置和博客设置 这两个文件主要是个人简介功能和博客头部个人签名设置 这两个文件在\_data目录下. 更改后author.yml和blog.yml内容如下:
 *  author.yml 不展示了 内容太多,而且大多数重复
 *  blog.yml:

![使用Jekyll 开发个人博客][Jekyll 8]

# 书写博客 #

 *  格式

在\_posts文件夹下新建文档,格式如下:\`yyyy-mm-dd-文件名称.md\` (文件名称可以带空格)

jekyll以日期+名称的格式命名文章,否则将不识别.

 *  文档头部

![使用Jekyll 开发个人博客][Jekyll 9]

其中:

![使用Jekyll 开发个人博客][Jekyll 10]

 *  kramdown kramdown遵循大多数markdown的语法,但是有一些细节的地方不太一致.比如说代码块和之前的文字描述之间必须有空行;Tab键和空格的表现形式差距很大等,
 *  push文章到仓库

将写好的文章push到远程仓库的gh-pages分支后,点击github分配给你的域名便可访问了.

# 个性域名设置. #

 *  申请域名: 我是从万网 申请的域名veryjava.cn. (已经归属于阿里云旗下) 具体申请步骤不再介绍.
 *  两种方式绑定个性域名:

 *  使用CNAME文件. 在博客文件夹的根目录创建CNAME文件.

![使用Jekyll 开发个人博客][Jekyll 11]

编辑CNAME文件放置自己的个性域名以及编辑后的内容

![使用Jekyll 开发个人博客][Jekyll 12]

是的,仅仅就只有一行,并且不要带www

然后将CNAME文件push到远程仓库就可以了.当然不要忘记设置域名的DNS解析.

 *  直接在仓库的设置选项中设置打开仓库设置选项 找到\`Custom domain\`功能,添加自己的域名即可.
 *  需要注意的地方 DNS解析到国外需要24小时的时间,所以设置好后,不要在看到仓库设置中的黄色警告时着急.等等明天就好了.

# 写在最后 #

为了生活感觉自己好累.从上海回到济南,为了未婚妻,一年半以后又从济南回到老家,一个5线城市里.生活经历了各种大起大落,回到家半个月了,未婚妻还没有正儿八经的跟我说过话.我的内心...不想说什么了.小30的人了,别那么想不开/cy.  



[Jekyll]: /pro/os/crawler/6FQB-J3QE-Q7NM.jpg
[Jekyll 1]: /pro/os/crawler/AYQV-ZFNY-2E6B.jpg
[Jekyll 2]: /pro/os/crawler/RZIA-IRRF-ARYJ.jpg
[Jekyll 3]: /pro/os/crawler/JMUA-UJNF-Y2QV.jpg
[Jekyll 4]: /pro/os/crawler/QFEN-A3RV-JIUZ.jpg
[Jekyll 5]: /pro/os/crawler/U2UF-NUYA-MU3Q.jpg
[Jekyll 6]: /pro/os/crawler/IIMA-6F2E-E3ER.jpg
[Jekyll 7]: /pro/os/crawler/FIJ7-R3RN-ININ.jpg
[Jekyll 8]: /pro/os/crawler/BYJY-UNBA-VAJJ.jpg
[Jekyll 9]: /pro/os/crawler/UIEI-2M2I-VIEB.jpg
[Jekyll 10]: /pro/os/crawler/AN7F-JAZR-YZRB.jpg
[Jekyll 11]: /pro/os/crawler/BBJM-UU73-UVBE.jpg
[Jekyll 12]: /pro/os/crawler/JRQY-EMIQ-Y7VN.jpg
 *  **原文作者：** 程序猿和程序媛的故事
 *  **原文链接：** https://www.toutiao.com/item/6401778291178471938/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。