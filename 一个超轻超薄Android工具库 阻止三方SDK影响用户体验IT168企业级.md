---
title: 一个超轻超薄Android工具库 阻止三方SDK影响用户体验
date: 2017-03-28 20:00:05
categories: "开发"
tags:
	- 科技
	- 软件
	- GitHub
	- CPU

---

Github开源贡献者oasisfeng开源了一个超轻薄Android工具库，阻止三方SDK中常见的严重影响用户体验的『链式唤醒』行为。（对应用自身的功能无影响）。![一个超轻超薄Android工具库 阻止三方SDK影响用户体验][Android_ _SDK]

github库  


它一个超轻超薄的Android工具库，将它套在Android应用工程里裸露的Context上，再传入第三方SDK（通常是其初始化方法），即可防止三方SDK中常见的损害用户体验的行为。

在后台启动大量其它应用的进程（在三方推送SDK中较为常见），导致应用启动非常缓慢，启动后一段时间内出现严重的卡顿（在中低端机型上尤其明显）。这是由于在这些SDK初始化阶段启动的其它应用中往往也存在三方SDK的类似行为，造成了进程启动的『链式反应』，在短时间内消耗大量的CPU、文件IO及 内存资源，使得当前应用所能得到的资源被大量挤占（甚至耗尽）。

**快速开始**

常见的三方SDK需要应用在启动阶段调用其初始化方法，一般包含**Context**参数，例如：

**XxxClient.registerXxx(context, ...);**

只需将其修改为：

**XxxClient.registerXxx(CondomContext.wrap(context, "XxxSDK"), ...);**

其中参数**tag**（上例中的"XxxSDK"）为开发者根据需要指定的用于区分多个不同**CondomContext**实例的标识，将出现在日志的TAG后缀。 如果只有一个**CondomContext**实例，或者不需要区分，则传入null亦可。

就这样简单的一行修改，三方SDK就无法再使用这个套上了保险套的**Context**去唤醒当前并没有进程在运行的其它app。 （已有进程在运行中的app仍可以被关联调用，因为不存在大量进程连锁创建的巨大资源开销，因此是被允许的。这也是Android O开始实施的限制原则）

**Github地址库：https://github.com/oasisfeng/condom**


[Android_ _SDK]: /pro/os/crawler/BRYR-7FZ2-YZRJ.jpg
 *  **原文作者：** IT168企业级
 *  **原文链接：** https://www.toutiao.com/item/6402518078638260738/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。