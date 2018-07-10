---
title: 使用vue-cli搭建vue项目完整版
date: 2017-10-24 14:58:03
categories: "开发"
tags:
	- 技术
	- Node.js
	- 电子商务

---

一、vue介绍

vue是一个用来构建用户界面的框架，vue只专注于视图层，属于主流的MVVM前端框架，关于MVVM的具体介绍不在本章讨论范围之内。它很方便的与现有的第三方组件库或现有项目进行集成，另外vue提供各种脚手架模块来方便项目的开发，本章讲解的vue-cli就是其中之一。

二、vue-cli介绍

vue-cli是vue提供的一款脚手架，集成了webpack环境及主要的依赖，对于项目的搭建、打包、维护等都非常方便快捷。它的特点有：

1、有一套成熟的vue项目架构设计，能够快速初始化一个vue项目。

2、vue-cli是官方支持的一个脚手架，维护性比较好。

3、vue-cli提供了一套本地的node测试服务器，使用vue-cli自己提供的命令，就可以启动服务器。

4、集成打包上线方案。

5、还有一些优点，包括：模块化，转译，预处理，热加载，静态检测和自动化测试等，等大家深入使用下去就会发现vue-cli的强大之处。

三、开始

1、安装nodejs

在nodejs官网下载最新版本的nodejs，需要选择4.0以上版本。安装完成之后输入node -v查看安装是否成功。

如果本机已经安装老版本的nodejs，建议安装nvm或者n模块进行切换node版本，具体的nvm的使用自行搜索。

2、配置淘宝npm镜像

由于需要通过npm下载各种模块，默认npm是从国处服务器下载，对于网络条件一般的用户可能很难下载成功，所以需要配置成淘宝的镜像。

打开命令行，输入：npm config set registry https://registry.npm.taobao.org

以后下载模块会自动从淘宝地址进行下载。

3、安装webpack

打开命令行，输入npm install webpack -g。安装完成后输入webpack -v，如果显示版本号，则说明安装成功。

4、安装vue-cli

打开命令行，输入npm install vue-cli -g。安装完成后输入vue -V查看版本事情，现在应该是2.8.2版本。

5、通过vue-cli初始化项目

打开命令行，进入指定的目录，输入vue init webpack 项目名，一路回车即可

![使用vue-cli搭建vue项目完整版][vue-cli_vue]

成功之后如图：

![使用vue-cli搭建vue项目完整版][vue-cli_vue 1]

项目建好之后还不能运行，因为还需要安装各种依赖，进入项目目录中，在命令行输入npm install -d即可，安装依赖需要大概3~5分钟。至此通过vue-cli构建项目成功，项目目录结构如下：

![使用vue-cli搭建vue项目完整版][vue-cli_vue 2]

7、启动项目

在命令行中输入npm run dev启动服务，启动完成后会默认打开一个欢迎页面，默认端口是8080，如图：

![使用vue-cli搭建vue项目完整版][vue-cli_vue 3]

8、结束  


至止整个构建、安装、运行过程已经完成，如果大家在过程中出现错误，一般需要检查本地nodejs的版本是不是比较新的版本，另外检查网络联通情况，大部分可能在npm install webpack和npm install这两个步骤会出现错误，具体错误大家可以在网上搜索。接下来的文章会主要介绍使用vue来开发项目。


[vue-cli_vue]: /pro/os/crawler/JAAI-UFEN-ZIB2.jpg
[vue-cli_vue 1]: /pro/os/crawler/YQUZ-AV3E-Y7VI.jpg
[vue-cli_vue 2]: /pro/os/crawler/F36V-32QV-N7ZR.jpg
[vue-cli_vue 3]: /pro/os/crawler/R22Y-YQB7-ZENZ.jpg
 *  **原文作者：** 极客殿堂
 *  **原文链接：** https://www.toutiao.com/item/6480368128885785101/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。