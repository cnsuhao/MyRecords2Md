---
title: 一种极低成本的Android屏幕适配方式
date: 2018-05-23 14:51:22
categories: "开发"
tags:
	- 数码

---

在Android开发中，由于Android碎片化严重，屏幕分辨率千奇百怪，而想要在各种分辨率的设备上显示基本一致的效果，适配成本越来越高。虽然Android官方提供了dp单位来适配，但其在各种奇怪分辨率下表现却不尽如人意，因此下面探索一种简单且低侵入的适配方式。

# **传统dp适配方式的缺点** #

android中的dp在渲染前会将dp转为px，计算公式：

 *  px = density \* dp;
 *  density = dpi / 160;
 *  px = dp \* (dpi / 160);

而dpi是根据屏幕真实的分辨率和尺寸来计算的，每个设备都可能不一样的。

**屏幕尺寸、分辨率、像素密度三者关系**

通常情况下，一部手机的分辨率是宽x高，屏幕大小是以寸为单位，那么三者的关系是：

![一种极低成本的Android屏幕适配方式][Android]

举个例子：屏幕分辨率为：1920\*1080，屏幕尺寸为5吋的话，那么dpi为440。

![一种极低成本的Android屏幕适配方式][Android 1]

**这样会存在什么问题呢？**

假设我们UI设计图是按屏幕宽度为360dp来设计的，那么在上述设备上，屏幕宽度其实为1080/(440/160)=392.7dp，也就是屏幕是比设计图要宽的。这种情况下， 即使使用dp也是无法在不同设备上显示为同样效果的。 同时还存在部分设备屏幕宽度不足360dp，这时就会导致按360dp宽度来开发实际显示不全的情况。

而且上述屏幕尺寸、分辨率和像素密度的关系，很多设备并没有按此规则来实现， 因此dpi的值非常乱，没有规律可循，从而导致使用dp适配效果差强人意。

# **探索新的适配方式** #

**梳理需求**

首先来梳理下我们的需求，一般我们设计图都是以固定的尺寸来设计的。比如以分辨率1920px \* 1080px来设计，以density为3来标注，也就是屏幕其实是640dp \* 360dp。如果我们想在所有设备上显示完全一致，其实是不现实的，因为屏幕高宽比不是固定的，16:9、4:3甚至其他宽高比层出不穷，宽高比不同，显示完全一致就不可能了。但是通常下，我们只需要以宽或高一个维度去适配，比如我们Feed是上下滑动的，只需要保证在所有设备中宽的维度上显示一致即可，再比如一个不支持上下滑动的页面，那么需要保证在高这个维度上都显示一致，尤其不能存在某些设备上显示不全的情况。同时考虑到现在基本都是以dp为单位去做的适配，如果新的方案不支持dp，那么迁移成本也非常高。

因此，总结下大致需求如下：

1.  支持以宽或者高一个维度去适配，保持该维度上和设计图一致；
2.  支持dp和sp单位，控制迁移成本到最小。

**找兼容突破口**

从dp和px的转换公式 ：px = dp \* density

可以看出，如果设计图宽为360dp，想要保证在所有设备计算得出的px值都正好是屏幕宽度的话，我们只能修改 density 的值。

通过阅读源码，我们可以得知，density 是 DisplayMetrics 中的成员变量，而 DisplayMetrics 实例通过 Resources\#getDisplayMetrics 可以获得，而Resouces通过Activity或者Application的Context获得。

先来熟悉下 DisplayMetrics 中和适配相关的几个变量：

 *  DisplayMetrics\#density 就是上述的density
 *  DisplayMetrics\#densityDpi 就是上述的dpi
 *  DisplayMetrics\#scaledDensity 字体的缩放因子，正常情况下和density相等，但是调节系统字体大小后会改变这个值

**那么是不是所有的dp和px的转换都是通过 DisplayMetrics 中相关的值来计算的呢？**

首先来看看布局文件中dp的转换，最终都是调用 TypedValue\#applyDimension(int unit, float value, DisplayMetrics metrics) 来进行转换:

![一种极低成本的Android屏幕适配方式][Android 2]

这里用到的DisplayMetrics正是从Resources中获得的。

再看看图片的decode，BitmapFactory\#decodeResourceStream方法:

![一种极低成本的Android屏幕适配方式][Android 3]

可见也是通过 DisplayMetrics 中的值来计算的。

当然还有些其他dp转换的场景，基本都是通过 DisplayMetrics 来计算的，这里不再详述。因此，想要满足上述需求，我们只需要修改 DisplayMetrics 中和 dp 转换相关的变量即可。

**最终方案**

下面假设设计图宽度是360dp，以宽维度来适配。

那么适配后的 density = 设备真实宽(单位px) / 360，接下来只需要把我们计算好的 density 在系统中修改下即可，代码实现如下：

![一种极低成本的Android屏幕适配方式][Android 4]

同时在 Activity\#onCreate 方法中调用下。代码比较简单，也没有涉及到系统非公开api的调用，因此理论上不会影响app稳定性。

于是修改后上线灰度测试了一版，稳定性符合预期，没有收到由此带来的crash，但是收到了很多字体过小的反馈：

![一种极低成本的Android屏幕适配方式][Android 5]

原因是在上面的适配中，我们忽略了DisplayMetrics\#scaledDensity的特殊性，将DisplayMetrics\#scaledDensity和DisplayMetrics\#density设置为同样的值，从而某些用户在系统中修改了字体大小失效了，但是我们还不能直接用原始的scaledDensity，直接用的话可能导致某些文字超过显示区域，因此我们可以通过计算之前scaledDensity和density的比获得现在的scaledDensity，方式如下：

![一种极低成本的Android屏幕适配方式][Android 6]

但是测试后发现另外一个问题，就是如果在系统设置中切换字体，再返回应用，字体并没有变化。于是还得监听下字体切换，调用 Application\#registerComponentCallbacks 注册下 onConfigurationChanged 监听即可。

因此最终方案如下：

![一种极低成本的Android屏幕适配方式][Android 7]

当然以上代码只是以设计图宽360dp去适配的，如果要以高维度适配，可以再扩展下代码即可。

# **Showcase** #

适配前后和设计图对比：

![一种极低成本的Android屏幕适配方式][Android 8]

适配后各机型的显示效果：

![一种极低成本的Android屏幕适配方式][Android 9]


[Android]: /pro/os/crawler/ER3Y-N2U7-ZYEN.jpg
[Android 1]: /pro/os/crawler/QNQU-VJMY-NUMZ.jpg
[Android 2]: /pro/os/crawler/RMEF-2ANZ-YYMN.jpg
[Android 3]: /pro/os/crawler/JMZ2-226F-JMMB.jpg
[Android 4]: /pro/os/crawler/FFFV-FRQZ-ZVNZ.jpg
[Android 5]: /pro/os/crawler/EQQQ-JYYA-QJJE.jpg
[Android 6]: /pro/os/crawler/ZZFB-FFJE-RVRM.jpg
[Android 7]: /pro/os/crawler/MIMI-MFNR-IMIR.jpg
[Android 8]: /pro/os/crawler/ZAVV-ENBU-FVQB.jpg
[Android 9]: /pro/os/crawler/IYNZ-ZVAI-BQNV.jpg
 *  **原文作者：** 今日头条技术团队
 *  **原文链接：** https://www.toutiao.com/item/6558665383790772739/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。