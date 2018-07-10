---
title: 你可以用Vue.js CLI做4件很棒的事情，尝试一下创建一些新的东西
date: 2018-04-25 12:56:57
categories: "开发"
tags:
	- 脚本语言
	- 路由器
	- 软件
	- HTML
	- JSON

---

到目前为止，您可能已经听说过Vue。js是前端世界中相对较新的孩子，在过去的几年中，他们一直被Angular和React 所主导。事实上，你们中的许多人可能会强烈地认同其中的Angular或React 作为你们的选择框架。虽然我从来没有试图说服你放弃这种偏好，但我确实认为你应该考虑添加Vue。js到你的工具带，即使只是为了原型设计和尝试新想法。


如果您正在计划一个Vue项目，我们建议您回顾一下“计划一个Vue应用程序”，在您开始之前编写的白皮书Brandon Satrom。

![你可以用Vue.js CLI做4件很棒的事情，尝试一下创建一些新的东西][Vue.js CLI_4]

关于Vue有很多东西值得你去喜爱。我个人最喜欢的一个是CLI，它提供了一些很酷的特性，可以定制新的项目、原型设计、添加插件，以及在不需要返回的情况下检查Webpack配置。这里有一个快速的纲要。

# 首先，安装Vue CLI #

在我们开始之前，如果您想要在家里继续，您将需要安装Vue CLI。这将是你一整天都做的最简单的事情，甚至比刷牙、清理垃圾更容易。

打开一个终端窗口，输入以下内容：

1.  npm install -g @vue.cli

或者，如果你更喜欢Yarn作为你的package manager：

1.  yarn global add @vue/cli

对于这篇文章，我假设您已经安装了Vue CLI的版本3或更高版本。如果您不确定您有哪个版本，请输入以下命令：

1.  vue --version

如果它小于版本3，运行上面的一个命令来安装最新的版本。

现在，您已经准备好与Vue一起摇摆了。

# 自定义您的项目 #

与 Angular和React (create-react-app) CLIs一样，Vue的CLI使创建新应用程序变得更加容易。Vue的方法最酷的特性是，您可以根据您想要做的事情来定制样板项目。

让我们试一试。在您的终端输入以下内容：

1.  vue create my-app

CLI会问你的第一件事是，如果你想要使用它的一个预设值，或者手动选择你想要使用的特性。如果您选择手动，您将看到以下屏幕。

![你可以用Vue.js CLI做4件很棒的事情，尝试一下创建一些新的东西][Vue.js CLI_4 1]

想要使用Vue的TypeScript？酷。想要建立一个PWA，这几天都很流行吗？这是一个模板。想要Vue路由器、Vuex用于状态管理和一些测试样板文件吗？完成了。

尝试使用空格键选择一些特性，然后点击回车。下一个提示将要求您根据所选择的特性做出一些其他的选择。我为我的项目挑选了所有的东西，所以下面的图片显示了你可能会被问到的所有问题。

![你可以用Vue.js CLI做4件很棒的事情，尝试一下创建一些新的东西][Vue.js CLI_4 2]

一旦你回答了所有问题，Vue就会下载并安装你需要的所有东西。从那里，您可以cd进入目录，并运行“npm run serve”，以查看项目或在编辑器中打开它。下面的图片显示了如果您选择了许多或全部可用特性，您的脚手架项目可能会是什么样子。

![你可以用Vue.js CLI做4件很棒的事情，尝试一下创建一些新的东西][Vue.js CLI_4 3]

# 轻松的原型 #

vue创建对于搭建一个完整的应用程序来说是非常棒的，它已经为严肃的开发做好了准备，但有时您只是想构建一个快速的原型，并且您想要快速创建一些东西，而不需要在过程中添加一些样板文件。

Vue的美妙之处在于，您可以创建一个HTML文件，为Vue添加一个脚本标记，并开始编写代码，但是Vue CLI有一个更快的特性，其中包括一个用于运行原型的开发服务器。

首先安装Vue CLI服务，并使用以下命令：

1.  npm install -g @vue/cli-service-global

然后，用.Vue扩展创建一个文件，并向它添加一些Vue代码。如果你想的话，你也可以从命令行中轻松地做到这一点：

1.  echo'<template><h1>Hello Vue.js CLI</h1><p>this is cool</p></template>' > App.vue

然后，您可以运行vue服务，并在实际操作中看到您的快速原型！

# 动态添加插件 #

通常，在我们开始之前，我们不知道我们需要的所有特性。例如，您可能认为您的应用程序中需要Cypress for end-to-end，但是您不确定，您也不希望从一开始就将依赖项添加到您的项目中。

值得庆幸的是，Vue CLI使您可以很容易地将这些插件添加到您的应用中，即使您在Vue创建过程中跳过它们。

首先，你需要在你的应用的根目录下运行以下命令来添加这个插件：

1.  npm install @vue/cli-plugin-e2e-cypress

当安装完成后，您可以使用invoke命令来完成您需要用Cypress进行测试的所有东西：

1.  vue invoke e2e-cypress

这个插件将添加依赖项和新文件和文件夹来进行测试。它还会向你的包添加一些脚本条目。用于端到端测试的json文件。通过运行npm运行e2e来尝试一下！

![你可以用Vue.js CLI做4件很棒的事情，尝试一下创建一些新的东西][Vue.js CLI_4 4]

# 在不弹出的情况下检查Webpack配置 #

与create-react-app类似，Vue CLI在Webpack周围创建了一个abstraction，它使您能够在不手动修改Webpack配置的情况下使用特性和依赖项。然而，想要对该配置进行调整并不少见，而且没有CLI能够预测您可能想要使用或修改的所有特性。

为了达到这一目的，Vue CLI允许您查看Webpack配置，并查看CLI生成的内容，如果您正在进行更改，并希望确保输出是您所期望的，那么这将是很有帮助的。在任何Vue CLI-generated项目中，只需运行以下命令：

1.  vue inspect

默认情况下，将配置输出到控制台，但您可以将其指向一个文件，如下所列：

1.  vue inspect > webpack.config.js

你也可以通过在点上的路径来检查配置的一部分：

1.  vue inspect resolveLoader.modules

请注意，此命令仅用于检查。您对生成的文件所做的任何更改都不会影响Vue在运行Vue服务或Vue构建时所依赖的配置。

Vue。js可能是这个街区的新（有点）的孩子，但不可否认的是，它有一些很酷炫的工具。无论您的 library还是选择的 framework，我建议您尝试一下，并在今天的Vue中创建一些新的东西。


[Vue.js CLI_4]: /pro/os/crawler/QZZQ-F3U2-6FY2.jpg
[Vue.js CLI_4 1]: /pro/os/crawler/J2AR-3YZ3-MEJA.jpg
[Vue.js CLI_4 2]: http://p1.pstatp.com/large/pgc-image/1524632024468693e137511
[Vue.js CLI_4 3]: http://p3.pstatp.com/large/pgc-image/152463203978851060b9ce7
[Vue.js CLI_4 4]: http://p1.pstatp.com/large/pgc-image/152463213634919dd7f896f
 *  **原文作者：** 空空教你玩黑客
 *  **原文链接：** https://www.toutiao.com/item/6548245512431075854/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。