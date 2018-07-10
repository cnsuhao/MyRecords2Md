---
title: 滴滴宣布开源 Android 端插件化框架 VirtualAPK
date: 2017-06-30 13:14:55
categories: "开发"
tags:
	- Uber
	- 移动互联网
	- Gradle
	- vivo
	- 滴滴打车

---

滴滴于今天正式宣布开源其 Android 插件化框架 —— VirtualAPK ，这也是滴滴公司的首个对外开源项目。

滴滴表示于去年开始研究 Android 插件化方面的技术，经过半年的开发、测试、适配和线上验证，最终形成了现在的 VirtualAPK 。VirtualAPK 已在滴滴内部得到了很好的验证，并已应用于滴滴乘客端和优步中国 APP 中。

VirtualAPK 主要有如下几个特性。

功能完备

 *  支持几乎所有的 Android 特性；
 *  四大组件方面

四大组件均不需要在宿主 manifest 中预注册，每个组件都有完整的生命周期。

1.  Activity：支持显示和隐式调用，支持 Activity 的 theme 和 LaunchMode，支持透明主题；
2.  Service：支持显示和隐式调用，支持 Service 的 start、stop、bind 和 unbind，并支持跨进程 bind 插件中的 Service；
3.  Receiver：支持静态注册和动态注册的 Receiver；
4.  ContentProvider：支持 provider 的所有操作，包括 CRUD 和 call 方法等，支持跨进程访问插件中的 Provider。
5.  自定义 View：支持自定义 View，支持自定义属性和 style，支持动画；
6.  PendingIntent：支持 PendingIntent 以及和其相关的 Alarm、Notification 和 AppWidget；
7.  支持插件 Application 以及插件 manifest 中的 meta-data；
8.  支持插件中的 so。

优秀的兼容性

 *  兼容市面上几乎所有的 Android 手机，这一点已经在滴滴出行客户端中得到验证；
 *  资源方面适配小米、Vivo、Nubia 等，对未知机型采用自适应适配方案；
 *  极少的 Binder Hook，目前仅仅 hook 了两个 Binder：AMS 和 IContentProvider，hook 过程做了充分的兼容性适配；
 *  插件运行逻辑和宿主隔离，确保框架的任何问题都不会影响宿主的正常运行。

入侵性极低

 *  插件开发等同于原生开发，四大组件无需继承特定的基类；
 *  精简的插件包，插件可以依赖宿主中的代码和资源，也可以不依赖；
 *  插件的构建过程简单，通过 Gradle 插件来完成插件的构建，整个过程对开发者透明。

![滴滴宣布开源 Android 端插件化框架 VirtualAPK][Android _ VirtualAPK]


[Android _ VirtualAPK]: /pro/os/crawler/AZFB-VJII-U6BE.jpg
 *  **原文作者：** 安安搞笑
 *  **原文链接：** https://www.toutiao.com/item/6437295671782408706/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。