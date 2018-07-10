---
title: Android 6.0运行时权限处理，不依赖外库 青年节特辑
date: 2017-05-04 09:48:22
categories: "开发"
tags:
	- 科技

---

现在Android 6.0以上设备越来越多了，所以Android 6.0 权限适配是必不可少的工作，这里主要介绍一下我是如何做Android 6.0权限适配的。当然也有很多优秀的开源库供大家选择，后面也给大家推荐一个，首先我们先要了解哪些权限需要在运行的时候去申请:

**一、 运行时权限分类**

Android 6.0 以上的权限变成了运行时权限，在需要使用某个权限的时候必须动态去申请使用，直接访问直接导致app崩溃!权限分为一般权限和危险权限两种,一般权限跟以前一样在AndroidManifest声明,危险权限需要开发者在代码中手动的动态申请.

危险权限包含:传感器、日历、摄像头、通讯录、地理位置、麦克风、电话、短信、存储空间,具体见下图:

![Android 6.0运行时权限处理，不依赖外库 青年节特辑][Android 6.0_]  


危险权限列表

除上面的危险权限之外的权限即为一般权限(普通权限)。

**二、程序中申请权限**

了解完之后，我们就来看看如何用代码编写权限申请.

(1)、新建一个BaseActivity类，所有activity均继承此类。

(2)、BaseActivity内的代码编写

**首先判断是否有相应的权限**

  
![Android 6.0运行时权限处理，不依赖外库 青年节特辑][Android 6.0_ 1]

**其次获取指定的权限**

![Android 6.0运行时权限处理，不依赖外库 青年节特辑][Android 6.0_ 2]

**最后处理请求结果**

![Android 6.0运行时权限处理，不依赖外库 青年节特辑][Android 6.0_ 3]

(2)、使用

![Android 6.0运行时权限处理，不依赖外库 青年节特辑][Android 6.0_ 4]

效果图:

![Android 6.0运行时权限处理，不依赖外库 青年节特辑][Android 6.0_ 5]

**三、推荐几个开源库的介绍:**  


想图省事的话，直接使用开源库也行，这里推荐几个还不错的

(1)、**PermissionsDispatcher**,地址:https://github.com/hotchemi/PermissionsDispatcher

(2)、**AndPermission**，地址:http://blog.csdn.net/qq\_33923079/article/details/53428756

(3)、**PermissionGen**，地址:https://github.com/lovedise/PermissionGen

好了，关于权限处理到这里就完毕了，其实很简单的几步,祝大家青年节快乐,快乐编程，快乐调BUG.


[Android 6.0_]: /pro/os/crawler/FQNI-2IMN-BZN2.jpg
[Android 6.0_ 1]: /pro/os/crawler/QUJ3-YBMF-EURA.jpg
[Android 6.0_ 2]: /pro/os/crawler/UNEN-IEF6-7FVV.jpg
[Android 6.0_ 3]: /pro/os/crawler/NNN7-ZF3M-Z6B2.jpg
[Android 6.0_ 4]: /pro/os/crawler/EURQ-AI3Y-EBJF.jpg
[Android 6.0_ 5]: /pro/os/crawler/VQNV-6FIR-6V7F.jpg
 *  **原文作者：** 图开森
 *  **原文链接：** https://www.toutiao.com/item/6416090589862822402/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。