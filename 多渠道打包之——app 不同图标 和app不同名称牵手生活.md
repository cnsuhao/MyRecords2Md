---
title: 多渠道打包之——app 不同图标 和app不同名称
date: 2017-08-25 11:29:54
categories: "开发"
tags:
	- 今日头条
	- 文章
	- 软件
	- Gradle
	- Android Studio

---

# 多渠道定义部分 #

在gradle文件中添加productFlavors 部分的定义

![多渠道打包之——app 不同图标 和app不同名称][app _ _app]

代码如下

productFlavors \{

WwStore\{

manifestPlaceholders = \[PalmvPlunChannel\_Value: "ic\_launcher\_wwStroe"

,app\_name: "@string/app\_name\_wwstroe", AppIcon: "@mipmap/ic\_launcher\_wwstroe"\] //

\}

NetworkSaleBox \{

manifestPlaceholders = \[PalmvPlunChannel\_Value: "ic\_launcher"

,app\_name: "@string/app\_name", AppIcon: "@mipmap/ic\_launcher" \] /

\}

\}

# 在图标资源文件路径下存放你不同渠道的图标 #

![多渠道打包之——app 不同图标 和app不同名称][app _ _app 1]

# 为不同渠道定义不同不同app\_name资源 #

![多渠道打包之——app 不同图标 和app不同名称][app _ _app 2]

# 在AndroidManifest.xml文件中配置实用对应的资源 #

![多渠道打包之——app 不同图标 和app不同名称][app _ _app 3]

# 注：在需要替换apk的label时，必须加上如下代码，否则会报错 #

tools:replace="android:label"

如果可以定义不同的包名，那么可以参考网友的这个文章：

android studio多渠道打包，定制个性化，替换不同资源文件，代码

参见：http://blog.csdn.net/u012761326/article/details/53673292

# 分享是一种美德，牵手是一种生活方式。 #

# 最后感谢今日头条提供的分享平台，你觉得有用可以收藏方便以后查阅。 #


[app _ _app]: /pro/os/crawler/MMRA-E2NJ-UZRJ.jpg
[app _ _app 1]: /pro/os/crawler/JFQR-JQZ2-QY2U.jpg
[app _ _app 2]: /pro/os/crawler/VIBQ-MA2U-JM2M.jpg
[app _ _app 3]: /pro/os/crawler/QBRQ-32I7-3AFU.jpg
 *  **原文作者：** 牵手生活
 *  **原文链接：** https://www.toutiao.com/item/6458048916196688398/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。