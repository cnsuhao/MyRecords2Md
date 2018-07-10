---
title: Android App增量，热修复，插件技术，让你的App维护起来更方便
date: 2017-10-23 20:05:02
categories: "开发"
tags:
	- DEX
	- 软件

---

前几天我们更新了，Android App项目后期的签名，打包，加固等技术，今天我们主要介绍一下，Android App上线后，App的增量更新技术，热修复技术和插件技术，这三项技术让我们的App上线后，维护更加方便，最方便的提高我们的开发效率。

首先我们从增量更新说起，什么是增量更新呢？我们先了解一下增量更新的原理：将手机上已安装 apk 与服务器端最新 apk 进行二进制对比， 得到差分包， 用户更新程序时， 只需要下载差分包， 并在本地使用差分包与已安装 apk， 合成新版 apk。例如， 当前手机中已安装微博 V1， 大小为 25.8MB， 现在微博发布了最新版 V2， 大小为 20.4MB， 我们对两个版本的 apk 文件差分比对之后， 发现差异只有 5.4M， 那么用户就只需要要下载一个 5.4M 的差分包，使用旧版 apk 与这个差分包， 合成得到一个新版本 apk， 提醒用户安装即可， 不需要整包下载 15.4M 的微博 V2 版 apk。

关于增量更新这里我们使用的是一个开源库：ApkPatchLibrary

使用过程大致分析，具体可参考库的具体使用方法.  


增量更新：下载增量 与本地的旧版本apk进行合成生成新apk;只是自载 .patch 服务器:生成 增量包 bsdiff.exe 客户端: 提交当前版本 5.5-->服务器 5.6 ==>patch

使用jni相关的库

static \{

System.loadLibrary("ApkPatchLibrary");

\}

PatchUitls:

public static native int patch(String oldApkPath, String newApkPath, String patchPath);

项目经验:

1.下载patch (Xutils httpclient okhttp)

2.创建AsynTask 调用PatchUitls.patch合成

3.调用ApkUtils(内部使用隐式意图调用安装界面)

![Android App增量，热修复，插件技术，让你的App维护起来更方便][Android App_App]

增量更新原理图

说完了，增量更新，我们再说一下，热修复技术，那么什么是热修复技术呢？热修复说白了就是”打补丁”，比如你们公司上线一个app，用户反应有重大bug,需要紧急修复。如果按照通 常做法,那就是程序猿加班搞定bug,然后测试,重新打包并发布。这样带来的问题就是成本高,效率低。于是,热 修复就应运而生.一般通过事先设定的接口从网上下载无Bug的代码来替换有Bug的代码。这样就省事多了,用 户体验也好。

![Android App增量，热修复，插件技术，让你的App维护起来更方便][Android App_App 1]

热修复原理图

目前较为成熟的热修复框架主要有AndFix、Nuwa以及微信的热更新思想。我们就拿AndFix简单举例，详细的可以参考具体的框架使用方法。

AndFix 具体的地址，因为是怕被和谐所以中间我就加中文，这是阿里热修复的具体Github地址：https://github.com/alibaba（阿里巴巴）/AndFix

步骤一、依赖 complie

步骤二、权限 INTERNET sd权限

步骤三、初始化Application

PatchManger.补丁管理者 load init add

步骤四、下载新的apatch

底层:.so --System.load(path) apk|dex|jar --- ClassLoader();

![Android App增量，热修复，插件技术，让你的App维护起来更方便][Android App_App 2]

获取热修复文件

apkpatch -f <new> -t <old> -o <output> -k <keystore> -p <\*\*\*> -a <alias> -e <\*\*\*>

apkpatch -f fix.apk -t bug.apk -o apatch -k android09.jks -p testtest -a testtest -e testtes

调用AndFix完成修复

    public class FixApp extends Application { @Override public void onCreate() { super.onCreate(); //初始化 补丁管理器PatchManager PatchManager patchManager=new PatchManager(this); String appversion="1.0"; patchManager.init(appversion); //加载修复文件:补丁 patchManager.loadPatch();//加载框架管理的修复文件。 //指定目录来加载 文件 apatch 在框架内必使用 apatch作为修复文件的扩展名 String path= Environment.getExternalStorageDirectory()+"/fixbug.apatch"; try { File pathFile=new File(path); if(pathFile.exists()) { patchManager.addPatch(path); } } catch (IOException e) { e.printStackTrace(); } }}

最后说一下Android 插件技术：安卓应用开发的大量难题，其实最后都需要插件技术去解决。现今插件技术的使用非常普遍，比如微信、QQ、淘宝、天猫、空间、携程、大众点评、手机管家等等这些大家在熟悉不过的应用都在使用。插件技术可以给项目开发带来巨大的好处，比如：并行高效开发、模块解耦、解除单个dex函数不能超过65535的限制、动态更新升级、按需加载等等。

插件化原理：Android有自己的ClassLoader，分别DexClassLoader和PathClassLoader，区别是PathClassLoader不能直接从zip包中得到dex，只支持直接操作dex文件或者已经安装过的apk。而DexClassLoader可以加载外部的apk、jar或dex文件，并且会在指定的outpath路径存放其dex文件。

以360的开源插件DroidPlugin为例简单讲解一下，具体想要了解的可以去官网研究怎么使用：

HOST程序：插件的宿主。插件：免安装运行的APK 完全模拟android的运行环境，在运行过程中就是依赖android运行APK的过程，只是我们在运行过程当中实时插入一些我们的逻辑，在运行过程中完全依赖android 自己的虚拟机，依赖android自己的运行流程或者完全模拟这样流程。插件有独立的classloader，与主程序与插件间是完全隔离，A插件无法访问B插件的代码，也没办法访问宿主的代码，在代码上实现完全隔离。插件实现资源隔离，资源ID不会冲突，每一个插件都用了独立的资源管理器，互相独立管理资源。插件框架模拟了一个android的运行环境，相当于对每一个插件分配了一个独立的数据目录，插件间没办法访问对方的数据，也没办法访问宿主的数据。插件运行过程中完全模拟了proccess的框架，插件APK本身有两进程，在运行当中，框架会模拟两进程，一般情况下插件运行在独立的进程中，崩溃不会影响宿主程序的。

每天坚持更新最新的技术分享~


[Android App_App]: /pro/os/crawler/3YVM-RIFJ-VMNF.jpg
[Android App_App 1]: /pro/os/crawler/JIN2-MJFQ-UU6R.jpg
[Android App_App 2]: /pro/os/crawler/AREU-BENZ-JJIB.jpg
 *  **原文作者：** 互联网生活那点事儿
 *  **原文链接：** https://www.toutiao.com/item/6480076153003442702/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。