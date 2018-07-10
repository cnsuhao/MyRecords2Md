---
title: 干货！javascript在移动端唤起应用的解决方案
date: 2018-01-27 18:04:31
categories: "开发"
tags:
	- 腾讯应用宝
	- Scheme
	- 移动互联网
	- JavaScript
	- 编程语言

---

# **背景** #

最近在做移动开发，碰到了在APP中唤醒微信、QQ、微博的需求，在顺利解决以后，小小整理了一下，希望把这个体验不错，功能完善的唤起技术分享给大家，希望对大家的开发有帮助。

# 需求 #

用户点击下载按钮或打开App的时候，判断如果用户已经安装了App，则根据业务跳转到相应的Native页面；如果用户没有安装，则跳转到AppStore或者应用市场去下载我们的App。

![干货！javascript在移动端唤起应用的解决方案][javascript]

首先所有的下载/唤起入口都是一个直接跳转，应该是这样：

![干货！javascript在移动端唤起应用的解决方案][javascript 1]

或是这样：

![干货！javascript在移动端唤起应用的解决方案][javascript 2]

所有的业务判断都是在“test”这个页面里面来做，这样有两个好处：

 *  多业务共用代码。在一个团队中，每个人的业务都可能有一个banner下载，没有比location到一个url更简单的调用方式了
 *  能够重复利用universal link

# universal link #

在iOS9之前，唤起方式和现在安卓是一个的，都是使用 scheme 进行唤起，这种方式有个问题：每次唤起，都会给个提示是否打开xx应用，这样从体验上来讲，又让用户多一步操作。universal link 会直接跳转，不会在页面做停留，条件就是在我们项目的根目录，增一个 apple-app-site-association.json 文件，里面的内容大致是这样：

![干货！javascript在移动端唤起应用的解决方案][javascript 3]

然后iOS的App后台再配置一下，就可以实现直接唤起了！

![干货！javascript在移动端唤起应用的解决方案][javascript 4]

经过长时间的实验，总结了这张在各种情况下，唤起成功/唤起失败的解决方案。

**微信**

微信是最重要的一种分享渠道，但是我们能够做的却不多。之前，ios 下的微信支持 universal link这种唤起方式，但是从2018年1月8日之后，**微信把这个给屏蔽了**！

不管微信基于什么原因，把 ios下这种最便捷的唤起方式屏蔽，我们能做的只能是适应。现在不管是iOS还是Andriod，我们的处理方式是一样的：**都是直接跳到应用宝**。iOS的应用宝会引导找开 AppStore，Andriod的应用宝会直接打开App（前提是你已经下载）

**注**：微信把iTunes 链接也屏蔽了，所以也没办法直接跳转 AppStore，只能借助应用宝来搭这个桥。

**微博**

微博目前还支持universal link 唤起，我们只需要考虑未下载的情况。

在ios下，微博是不支持打开应用宝的链接，所以我们需要引导用户使用Safari打开，像这样：

![干货！javascript在移动端唤起应用的解决方案][javascript 5]

在Android平台下，**使用 scheme这种方式是唤不起App的**，但是有特例，同样是scheme，大众点评和网易云音乐就可以唤起，所以我们可以推断出，安卓平台下的微博，也有类似微信一样的白名单，在白名单内的，就可以使用scheme
方式唤起的。

不管是ios还是Android，我们的方案是：**直接引导用户使用本地浏览器打开**。

**QQ**

#### ios平台下，QQ目前还支持universal link唤起，要是没有安装，QQ下也支持直接打开itunes链接，比较其他应用，QQ支持是最好的。 ####

#### 在android平台上，QQ也支持scheme方式唤起，但是**在一些老机型下，QQ会有一定的概率唤起失败。** ####

具体的现象是：第一次打开页面，唤起失败，再次打开，唤起成功。根据现象，我们可以推测出，在QQ的webview中，会对scheme的唤起方式做一些加载时间上的限制，经测试，大约在500ms。

为什么第二次打开，唤起成功的概率会大，是因为第一次加载时，已缓存了文件，第二次打开直接加载，这样时间在限制在500ms之内。

#### **Safari** ####

Safari这种情况比较简单，支持universal link，也支持直接打开itunes。

以上就是在实际开发中总结出来的一些经验了。希望能和大家一起交流学习进步。

我是一枚程序员，热爱生活，希望和大家一起学习进步。

![干货！javascript在移动端唤起应用的解决方案][javascript 6]


[javascript]: /pro/os/crawler/JN6F-BUVM-JF2I.jpg
[javascript 1]: /pro/os/crawler/QQUU-6NJI-VEFI.jpg
[javascript 2]: /pro/os/crawler/VA6V-AMII-3263.jpg
[javascript 3]: /pro/os/crawler/IUQB-BNBA-MFQU.jpg
[javascript 4]: /pro/os/crawler/AQFM-V3EN-JVZF.jpg
[javascript 5]: /pro/os/crawler/JEFJ-NJEU-R3MB.jpg
[javascript 6]: /pro/os/crawler/ENZA-U26J-VIIY.jpg
 *  **原文作者：** 荒唐鸟
 *  **原文链接：** https://www.toutiao.com/item/6515342055575650824/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。