---
title: 使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站
date: 2018-01-03 18:32:44
categories: "开发"
tags:
	- Pages
	- Origin
	- Line
	- GitHub
	- HTML

---

![使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站][Vue_Nuxt.js_Netlify_15]

这是一个快速教程目的是介绍如何使用vue.js，nuxt.js和netlify建立和运行一个静态网站。

# 简单的介绍 #

静态站点这个词不是新鲜词，概念也不是新概念。静态站点仅仅是一个网站，它提供给客户一个纯粹的静态资产，如HTML、CSS和JavaScript。没有数据库，没有PHP或Ruby on Rails，没有像Drupal CMS或WordPress这样的后台管理。基本上，浏览器请求网页的时间和接收网页的时间之间是没有计算的。

创建静态站点最快的方法是将一个**.html**文件保存在桌面上，并双击它！你的浏览器将打开文件并显示内部文本。这似乎是很容易的！！

在现实世界中，事情并非如此简单，但它也不是特别费劲。我将给你演示几个快速的步骤，让你自己的静态站点在15分钟或更少的时间内运行起来！

我们将使用一些最新的技术，使压缩、代码管理和开发变得轻而易举！我们要用Nuxt.js，平台建立在Vue.js的基础上，用它来开发整个网站。

# 入门 #

对于这个项目，你需要做以下事情：

1.  在本地安装Git
2.  一个Github帐号
3.  一个netlify帐户链接到你的GitHub
4.  在本地安装Node.js和npm

# 创建一个Nuxt.js项目 #

设置你的环境是建立一个**Nuxt.js**基础上样板项目的第一步。我们将使用**Vue-cli**轻松地创建一个“交钥匙”的应用程序，在其上我们将建立我们的网站。

**安装Vue-cli**

Vue-cli将使我们能够很容易地创建一个新的**nuxt**项目和设置（几乎）所有必要的参数。在终端中输入以下内容。

``````````
npm install vue-cli -g
``````````

**创建一个nuxt启动器**

一旦Vue-cli已经完成安装，你可以浏览到你想要的项目文件夹的根目录。通常情况下在你的主用户配置文件有一个**repos**目录。然后您可以在终端中键入以下内容：

``````````
vue init nuxt/starter <project-name># Follow the command line instructions# (most defaults should be fine) so just press enter a few timescd <project-name>npm install
``````````

请务必用您想要的站点项目名称替换**<project-name>**。如果你真的想做一个个人网站，试试“my-site”或者“my-portfolio.”，记住这个名字，因为它在以后的步骤中会用到。

**初始化本地git**

为了与GitHub配合使用，我们初始化我们的文件夹作为一个Git仓库，然后我们会链接到一个远程的GitHub主机存储库。将下列类型输入到终端（确保当前目录是**<project-name>项目**的根目录）。

``````````
git init # initializes gitgit add . # stages all files to be committedgit commit -m "Initial commit"
``````````

**在GitHub上创建一个空库**

下一步是在GitHub上创建一个空库。登录到github.com并选择“New repository”：

![使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站][Vue_Nuxt.js_Netlify_15 1]

![使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站][Vue_Nuxt.js_Netlify_15 2]

同前面的步骤中输入相同的项目名称

GitHub会问你的新repo的名字，你应该保持一致使用前面步骤的项目名称！你现在可以跳过添加许可证和readme，因为我们要发布我们的nuxt项目repo。所以只需单击“Create repository”。

在下一个屏幕上你会看到指示“push an existing project from the command line.”。我们将用这些来发布我们的项目到GitHub。回到你的终端输入通过GitHub提供给你的命令。他们应该看起来像这样：

``````````
git remote add origin git@github.com:Jexordexan/project-name.gitgit push -u origin master
``````````

push后，检查Github看看你的代码是否被发布了。

# 生成静态站点 #

在本节中，我们将生成我们的站点，并查看创建的内容。

**运行开发服务器**

当我们初始化项目，Vue-cli给我们提供了快速的创建我们的网站一些命令。第一个用于本地开发站点：

``````````
npm run dev
``````````

这将编译你所有的网页和文件并使用Webpack启动一个Web服务器。你现在可以用你的浏览器打开这个网址http://localhost:3000，看到该项目的初始屏幕。这只是一个占位符，您需要自定义此页面成为自己的站点。用你最喜欢的文本编辑器编辑index.html以便打开src/pages/index.vue。更改**template**标记中的任何内容并保存。你将立即看到你的更改显示在您的浏览器中。

创建一个新的页面，添加相同的**pages**目录与文件及扩展名为.vue的新文件。例如，如果你想在www.mysite.com/about有一个页面那就创建一个文件名为about.vue。然后将HTML放在文件顶部的<template></template>中。你可以阅读更多关于开发你的网站通过nuxt.js官网文档。

**生成静态站点代码**

Nuxt.js会自动将您的项目生成为一个静态的网站，可以分布在任何HTTP服务器。作为一种支持，它配备了很多前沿技术如**prefetch**和http2等！更好的是，它的使用就像在终端中输入一个命令一样简单：

``````````
npm run generate
``````````

你将看到叫**/dist的**一个新的文件夹生成在项目的根目录下。这是静态站点被放置的地方。随便看看它是如何组织的！所有的代码都是压缩的，但它却可以正常工作！

# **通过Netlify发布** #

现在你的代码很好用，但是它还在你的电脑上，别人看不见！让我们改变这一点！

**连接netlify到GitHub的repo**

为了让netlify获取到你的代码，它需要知道在哪里获取！这真的很容易做到。首先你需要登录到https://app.netlify.com。你用你的GitHub账号注册登录更方便。

接下来，找一个像这样的按钮：

![使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站][Vue_Nuxt.js_Netlify_15 3]

你可以在主菜单的右上角找到它

点击它，然后从连续部署选项列表中选择“GitHub”。

![使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站][Vue_Nuxt.js_Netlify_15 4]

下一步你需要给netlify访问您的存储库的权限，所以授权提示时，你应该看到你的所有repo。

从列表中选择我们创建的repo。

下一页说明netlify如何知道你的网站代码存在哪里，匹配下面的设置并按“Deploy site”

![使用Vue、Nuxt.js和Netlify在少于15分钟内构建一个静态网站][Vue_Nuxt.js_Netlify_15 5]

netlify生成设置

请注意“Build command”是如何生成本地站点的，而“Publish directory”是放置在哪里的？这些内容需要设置，因为netlify只是一直等待直到你生成代码并输出而不管结果是什么，然后把它们推到它的服务器上，你的站点就可以激活了！


netlify生成随机域主办的网站。如果你已经有了自己的域名，你需要遵循netlify相应的步骤设置DNS服务器链接。他们有一个简单的循序渐进的指南来帮助你做到这一点。

OK，你做到了！！

你的站点现在是实时在线的，任何人都可以看到！每当你push改变GitHub，netlify将生成网站并在主机上进行新的自动构建。我们生活在一个多么美好的世界里！

这篇文章是我在波士顿的聚会时jamstack随访的一个闪电演讲。如果你在波士顿，想学习更多关于jamstack技术，来和我们一起吧！

你可以看到这里看演示站点：https://palmist-selina-23424.netlify.com/

玩得开心，不要不好意思寻求别人帮助！

翻译**汇智网（www.hubwiz.com）**小智


[Vue_Nuxt.js_Netlify_15]: /pro/os/crawler/IRVE-MIMU-RZMI.jpg
[Vue_Nuxt.js_Netlify_15 1]: /pro/os/crawler/YVFZ-VZAU-BRUB.jpg
[Vue_Nuxt.js_Netlify_15 2]: /pro/os/crawler/Z7ZZ-ZAYM-UIYJ.jpg
[Vue_Nuxt.js_Netlify_15 3]: /pro/os/crawler/ZMIF-ZFVQ-YYEU.jpg
[Vue_Nuxt.js_Netlify_15 4]: /pro/os/crawler/ZQJN-ARUV-YAZ3.jpg
[Vue_Nuxt.js_Netlify_15 5]: /pro/os/crawler/BZQF-JZNE-YYZM.jpg
 *  **原文作者：** 编程狂魔
 *  **原文链接：** https://www.toutiao.com/item/6506770503431094792/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。