---
title: 善用Axure写PRD，把原型放到手机里查看
date: 2017-03-17 17:47:21
categories: "开发"
tags:
	- Pages
	- 程序员
	- 软件
	- GitHub
	- Axure

---

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD]

核心是应该是把原型发布到网络上。

一方面可以用手机查看原型并模拟用户使用APP的交互体验；另外一方面，让技术随时随地能够查看原型并写代码。

## 为什么选择Github ##

其实将原型发布到网络上的方法有很多种。

之所以选择Github，有以下几个原因：

 *  查看原型的主要是程序员，而大部分技术团队都在使用git或者svn来管理代码、你用github会让他们更认可你的能力。
 *  Github的大部分功能是免费的，足够PM展示原型的需求。
 *  如果新原型出错，Github可以让你随时回退原型到以前的版本。
 *  方便双方撕逼，因为每一次原型都存档并记录在服务器上。

## 注册Github ##

打开https://github.com，输入相关资料注册一下，或者右上角Sign up。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 1]

## 创建项目 ##

登录成功后，点击右上角的+或者new repository按钮，开始创建仓库，你可以认为那是一个项目。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 2]

## 下载和安装客户端 ##

点击set up desktop下载git客户端，然后安装并登录。

## 下载项目到本地 ##

clone你刚刚创建的项目到本地目录，很简单的。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 3]

## 发布原型到该目录 ##

打开Axure生成原型到该目录，注意原型的尺寸需要单独设置。详见上篇文章《善用Axure写PRD，如何设置手机APP原型尺寸》。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 4]

## 提交到服务端 ##

点击你的仓库，然后会显示哪些文件已修改，然后输入摘要和描述并Commit to master，最后Publish到服务端。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 5]

## 查看源代码 ##

右键左边的项目进入它对应的主页，此时项目变成了这样。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 6]

## 开启网页访问功能 ##

点击setting进入，然后页面往下拖动到GitHub Pages，将Source设为master branch并save。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 7]

## 得到原型的网址 ##

保存成功之后回到GitHub Pages，会显示一行网址https://wbfbest.github.io/quickmeeting/，这就是原型网址。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 8]

## 手机查看原型 ##

将该网址发布到手机上，然后关闭左下角的close，此时就可在手机浏览器上查看原型。当然最好生成到桌面查看效果。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 9]

## 比较原型版本 ##

如果有需要的话，可以比较原型的版本。

![善用Axure写PRD，把原型放到手机里查看][Axure_PRD 10]

## 总结 ##

网上有通过命令行来发布原型到git的文章并且太偏技术语言了。不太适合PM同学来学习使用。所以单独从可视化的角度来写了这篇文章。

另外，通过使用github可以了解程序员是如何提交代码合并分支，对PM是件好事。


[Axure_PRD]: /pro/os/crawler/UZNZ-BMFR-NQVB.jpg
[Axure_PRD 1]: /pro/os/crawler/RZQY-UZ2A-6BBQ.jpg
[Axure_PRD 2]: /pro/os/crawler/RZFN-JURA-FVMM.jpg
[Axure_PRD 3]: /pro/os/crawler/QV7R-3IEV-FVEM.jpg
[Axure_PRD 4]: /pro/os/crawler/MQ77-7JZY-YNQE.jpg
[Axure_PRD 5]: /pro/os/crawler/BIFM-MFYB-YBZE.jpg
[Axure_PRD 6]: /pro/os/crawler/JYQ3-6VEJ-ZAQR.jpg
[Axure_PRD 7]: /pro/os/crawler/ZY6F-BQEY-VYZY.jpg
[Axure_PRD 8]: /pro/os/crawler/BIAN-IUMR-RUQY.jpg
[Axure_PRD 9]: /pro/os/crawler/FIRV-IYVB-JIBV.jpg
[Axure_PRD 10]: /pro/os/crawler/QYZM-ZBFV-ZUIZ.jpg
 *  **原文作者：** 人人都是产品经理
 *  **原文链接：** https://www.toutiao.com/item/6398401946809532930/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。