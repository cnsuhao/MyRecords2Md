---
title: 开机自启动app的实现
date: 2017-06-19 11:22:41
categories: "开发"
tags:
	- 技术
	- 广播
	- 软件
	- GitHub

---

开机自动启动app的实现是比较简单的，监听一个开机广播即可。当监听到开机广播后打开想要启动的app即可，具体实现如下：

1. 创建广播接收器：BootBroadcastReceiver。

package cn.studyou.autoopenapp;

import android.content.BroadcastReceiver;

import android.content.Context;

import android.content.Intent;

import android.os.Handler;

/\*\*

\* 基本功能：开机自动启动APP

\* 创建：王杰

\* 创建时间：16/7/22

\* 邮箱：w489657152@gmail.com

\*/

public class BootBroadcastReceiver extends BroadcastReceiver \{

static final String ACTION = "android.intent.action.BOOT\_COMPLETED";

@Override

public void onReceive(Context context, Intent intent) \{

if (intent.getAction().equals(ACTION)) \{

final Intent mainActivityIntent = new Intent(context, MainActivity.class); // 要启动的Activity

mainActivityIntent.addFlags(Intent.FLAG\_ACTIVITY\_NEW\_TASK);

final Context mContext = context;

new Handler().postDelayed(new Runnable()\{

public void run() \{

mContext.startActivity(mainActivityIntent);

\}

\}, 10000);

\}

\}

\}

2. 在application声明Receiver。

    <receiver android:name=".BootBroadcastReceiver">

3. 声明权限。

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"></uses-permission>

4. 特殊说明：

1） Apk需要设置默认安装到手机内存，外设SD卡是接收不到开机广播，这里只针对安装位置为手机内存的app。

2）小米手机收不到开机广播的处理办法：系统与安全文件夹--->安全中心--->授权管理--->自启动管理--->对本App添加自启动授权

最后附上我的源码 github地址 包括原生定位，后台静默拍照录像，开启wifi热点，UDP通信，APP自启功能等 https://github.com/chenfei1988/dashcam

![开机自启动app的实现][app]


[app]: /pro/os/crawler/INBN-ZYER-7F73.jpg
 *  **原文作者：** android开发交流
 *  **原文链接：** https://www.toutiao.com/item/6433184814257406466/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。