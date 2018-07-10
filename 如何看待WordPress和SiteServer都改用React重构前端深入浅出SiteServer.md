---
title: 如何看待WordPress和SiteServer都改用React重构前端
date: 2017-07-17 06:55:17
categories: "开发"
tags:
	- WordPress
	- Linux
	- MySQL
	- Node.js
	- Windows

---

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React]

WordPress是以PHP和MySQL为核心的开源内容管理系统，曾是互联网上最流行的开源系统。Alexa排行前百万的网站中曾有超过16%使用它。

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React 1]

SiteServer CMS 是中国最强大的企业级开源CMS内容管理系统和网站群系统，能够以最低的成本、最少的人力投入在最短的时间内架设一个功能齐全、性能优异、规模庞大并易于维护的网站平台。

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React 2]

# 1、WordPress开源前端项目Calypso #

几年前，WordPress开始重新思考技术框架和流程。尽管原有的代码库和流程仍能可靠的工作，但十年来积累的各种历史遗留问题，令他们很难继续打造能跟上时代的现代、快速、移动优化这些当前用户所关注特性的产品。第三方开发者与设计师在也哑火了，不像过去那样，围绕WordPress的插件、主题层出不穷。

两年前，WordPress的母公司Automattic 收购了云存储应用公司Cloudup，后者有一套用JavaScript开发的API文件分享工具。Cloudup团队向WordPress展示了完全基于JavaScript打造一套产品的可能，并打动了他们。

到2015年中，Calypso代码库已足够完备，因为完全由JavaScript、HTML和CSS写成，因此可在Node.js服务器上运行。使用Electron，基于相同的代码库，他们已经发布了Mac、Windows和Linux客户端。

Calypso采用Node.js + React开发，WordPress并没有完全抛弃PHP，而是将其用于后端，前端使用Calypso，以拥抱今天的Web趋势。

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React 3]

# 2、SiteServer CMS 全新用户中心 #

SiteServer CMS 是一款拥有十年历史与广泛知名度的CMS系统，今天迈出了自成立以来的最具跨越性的一步，宣布开源并推出全新5.0版本。

SiteServer CMS宣布彻底开放源代码并融入开源社区，让开发人员可以更轻松地集成与定制系统。遵循GPL 开源许可，将所有源代码在 GitHub 上托管并开源（https://github.com/siteserver/cms）。你可以查看代码、建立自己的代码分支并且重新使用它，也可以加入SiteServer CMS开源团队，共同维护并完善产品。

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React 4]

SiteServer CMS 团队对5.0 版本进行了大刀阔斧的改造，从内到外一切焕然一新。尤其是前台用户中心，代码全部重写，不再沿用 ASP.NET，而是转用 JavaScript 和 API 调用，采用ReactJS 与Restful API来完成所有功能，使用户中心成为单页应用，这意味着更快速、更实时、响应更灵敏。

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React 5]

# 3、WordPress放弃PHP？SiteServer CMS放弃.NET? #

**Calypso 只是一个前端单页 Web 程序（single page web application，SPA）**，从它界面可以看到它是博主更新和管理博客内容的后台，并不涉及到 WordPress的核心。Calypso是基于WordPress的Restful API，Restful API是基于什么写的？当然还是**PHP+MySQL**。所以 **WordPress并没有放弃 PHP**，只是使用 NodeJS+React 重构了博主管理后台。

同理，**SiteServer CMS全新用户中心也只是一个SPA**，只是前台用户登录用户中心进行基本操作的一个界面，并不涉及到 SiteServer CMS的核心。用户中心是基于SiteServer CMS的Restful API，而Restful API还是基于**.NET**。所以 **SiteServer CMS也没有放弃.NET**，只是使用 NodeJS+React 重构了用户中心前台。

# 4、为什么选择NodeJS+React重构前端？ #

![如何看待WordPress和SiteServer都改用React重构前端][WordPress_SiteServer_React 6]

WordPress官方和SiteServer CMS官方都应该是充分看到了未来web在前端渲染上特别是SPA等等对用户体验将是一种质的飞跃，所以选用目前最为有前景的NodeJS + React生态无疑是个不错的选择。具体来说有以下好处：

 *  SPA在2010年开始流行（Backbone.js时代），主要好处就是反应快，所有的UI变化都在本地浏览器上完成，不需要重新去server请求一个页面，也不会有明显的页面跳转延时。
 *  和server的通信基于Restful API，传输的内容只是数据而已，没有了冗长的 html、css和javascript 的传输，所以能大大提升系统的性能。
 *  将前端逻辑和渲染做成独立的MVC结构，使其和其他移动端并列，用统一的restful API来提供服务。比如可以复用这一套 Restful API，用来给 IOS 和 Android 端以及PC端使用，大大节省开发时间，提高开发效率。
 *  React已经非常成熟，而且在Facebook内部各种项目都在使用。比如Facebook首页的整个search功能的UI都是react来完成的。React会比起一些业余时间完成的开源项目有保障很多。前段时间阿里还搞了一场开源项目之React技术栈在蚂蚁金服的实践大会。


[WordPress_SiteServer_React]: /pro/os/crawler/YFV6-JFM6-JJJV.gif
[WordPress_SiteServer_React 1]: /pro/os/crawler/RVJV-BNJM-UQMU.jpg
[WordPress_SiteServer_React 2]: /pro/os/crawler/AJEI-IIBE-ZYNQ.jpg
[WordPress_SiteServer_React 3]: /pro/os/crawler/FYYQ-AB6J-YJBZ.jpg
[WordPress_SiteServer_React 4]: /pro/os/crawler/N32A-QYZU-IYN2.jpg
[WordPress_SiteServer_React 5]: /pro/os/crawler/7V26-ZQJR-NQMV.jpg
[WordPress_SiteServer_React 6]: /pro/os/crawler/AE3A-RVZQ-BNJ3.jpg
 *  **原文作者：** 深入浅出SiteServer
 *  **原文链接：** https://www.toutiao.com/item/6443429186299232781/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。