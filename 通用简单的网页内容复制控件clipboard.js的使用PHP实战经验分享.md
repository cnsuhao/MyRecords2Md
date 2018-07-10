---
title: 通用简单的网页内容复制控件clipboard.js的使用
date: 2017-08-24 11:16:33
categories: "开发"
tags:
	- 科技
	- DIALOG
	- Flash

---

# 最近做的项目需要使用按钮复制功能，最后确定使用clipboard.js插件。原因：小巧，使用简单，不需要flash，纯js支持。 #

1.  首先去官网(https://clipboardjs.com)下载clipboard.js的插件，然后引入到自己的项目里；
2.  页面设置按钮，从服务器获取拼装好的url，然后返回；

![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js]

3. 获取链接后，如果返回成功，会调用弹窗，我这里用的是art-dialog插件；

![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 1]

4. 弹窗页面和代码如下；  


![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 2]

![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 3]

5. 点击“复制至剪贴板”会，调用copy\_text方法，该方法需要当前点击按钮的id或者class，因为这个按钮是插件后期生成的，所以在浏览器里查看得到class是ui-dialog-autofocus;  


![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 4]

6. 从官网下载的demo可以看到有很多的使用场景，我选的是function-text这个demo；

![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 5]

7. 结合项目的代码如下图；

![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 6]

8. 下图是复制后打印的log，实际复制的结果是text的值；

![通用简单的网页内容复制控件clipboard.js的使用][clipboard.js 7]

以上是浏览器内容复制的教程，是不是很简单！如果喜欢的话，请点击关注哦！  



[clipboard.js]: /pro/os/crawler/NYUB-VABF-FURV.jpg
[clipboard.js 1]: /pro/os/crawler/RAB3-YZYR-AMQV.jpg
[clipboard.js 2]: /pro/os/crawler/AZBN-UNR2-QABV.jpg
[clipboard.js 3]: /pro/os/crawler/AM2A-ERYI-MEBN.jpg
[clipboard.js 4]: /pro/os/crawler/Y2QR-2URI-QANU.jpg
[clipboard.js 5]: /pro/os/crawler/YVZR-6N2I-YURA.jpg
[clipboard.js 6]: /pro/os/crawler/JRAF-JZVU-JBVN.jpg
[clipboard.js 7]: /pro/os/crawler/JIAR-NANR-26RY.jpg
 *  **原文作者：** PHP实战经验分享
 *  **原文链接：** https://www.toutiao.com/item/6457674854878413326/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。