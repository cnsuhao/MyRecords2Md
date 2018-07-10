---
title: android性能测试工具：BlockCanaryEx
date: 2017-05-21 00:18:09
categories: "开发"
tags:
	- 科技
	- GitHub

---

今天分享一个android中的性能测试工具BlockCanaryEx。这是一个 帮助你在开发中找到应用中耗时较长的方法 的一个库，使用方法简单，个人觉得比AS自带的分析工具使用简单，直观。好不好用 先看看效果：

![android性能测试工具：BlockCanaryEx][android_BlockCanaryEx]

![android性能测试工具：BlockCanaryEx][android_BlockCanaryEx 1]

可以直观的看到每个方法执行的时间，方便找出找出应用卡顿的真凶。

集成方法如下：  


project build.gradle

![android性能测试工具：BlockCanaryEx][android_BlockCanaryEx 2]

model build.gradle

![android性能测试工具：BlockCanaryEx][android_BlockCanaryEx 3]

然后是初始化，在你的Applicaton中初始化

![android性能测试工具：BlockCanaryEx][android_BlockCanaryEx 4]

当然这只是初级的用法，默认的作用域是只有你src中的代码，外部库等一些都被忽略了，如果想要更改范围以观察更多的方法性能，您可以在分级中进行配置。具体方法如下：

在你的model build.gradle中

![android性能测试工具：BlockCanaryEx][android_BlockCanaryEx 5]

更多方法请参见github:https://github.com/seiginonakama/BlockCanaryEx


[android_BlockCanaryEx]: /pro/os/crawler/FBE7-R27N-IJIB.jpg
[android_BlockCanaryEx 1]: /pro/os/crawler/RB7N-RZMN-QYIY.jpg
[android_BlockCanaryEx 2]: /pro/os/crawler/3YJ2-QJAY-UF7F.jpg
[android_BlockCanaryEx 3]: /pro/os/crawler/ABRB-UVFQ-NYE2.jpg
[android_BlockCanaryEx 4]: /pro/os/crawler/ZFI3-UIBN-IFUQ.jpg
[android_BlockCanaryEx 5]: /pro/os/crawler/JII2-Q2JQ-UV7N.jpg
 *  **原文作者：** 网聚天下
 *  **原文链接：** https://www.toutiao.com/item/6422105932502663682/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。