---
title: 阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现
date: 2017-11-09 17:19:05
categories: "开发"
tags:
	- MP4
	- 移动互联网
	- FFmpeg
	- GitHub
	- Flash

---

阿里云播放器SDK（ApsaraVideo for Player SDK）是阿里视频云端到云到端服务的重要一环，除了支持点播和直播的基础播放功能外，还深度融合视频云业务，支持视频的加密播放、安全下载、首屏秒开、低延时等业务场景，为用户提供简单、快速、安全、稳定的视频播放服务。本文衔接上文，详细介绍web播放器的功能及实现。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web]

# 一、基本概况及功能 #

 *  播放器架构

Aliplayer Web播放器分为H5和Flash两个，Flash播放器随着技术的发展会逐渐被边缘化，所以我们以后只做维护，不会更新功能了，重点会放在H5播放器上。H5播放器架构主要分四层，底层H5 Video，播放能力和H5原生Video紧密相关。第二层是基础播放器，它不依赖于具体业务，通过URL的方式来播放。第三层是为各种业务场景准备的不同的播放器，可以很容易的扩展，相互隔离不依赖。最上面一层是适配的播放器，会根据终端类型、浏览器类型、播放格式和用户指定来进行智能适配。

 *  播放器功能

最近，我们在播放器端上也实现了截图、国际化、变速、UI自定义、微信同层播放、自适应播放、加密播放、H5播放flv、自定义插件等功能。后续，我们还会通过插件的形式实现弹幕、广告等功能，并会开源到github上，也会支持用户根据自己业务需求来自定义SDK包。

 *  播放器支持视频格式

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 1]

 *  适配播放

我们整个视频播放的基本原则是H5优先，能用H5播放的肯定不用Flash去播放。所以在移动端，我们肯定是用H5来播放的，PC端也依照这个原则尽量使用H5。同时，我们会判断浏览器类型支持哪种播放格式，比如m3u8在PC端IE11以上的浏览器才能播放，如果遇到IE11以下的浏览器，我们自动会选择Flash播放。在视频格式方面，假设视频是rtmp和flv，我们会自动选择Flash播放。另外，如果用户自主设置useH5Prism和useFlashPrism属性，那我们也会依照用户的选择。

 *  浏览器支持情况

FLASH支持IE8以上，在浏览器上启动允许FLASH运行即可；H5支持IE9以上，m3u8需要在IE11以上才可以运行；其他浏览器都也都是可以支持的。

 *  两种播放方式

1.  source，通过url 去播放
2.  通过点播vid+playauth去播放，第二种方式和视频云结合比较紧密

 *  点播播放格式的选择

点播服务中转码生成的视频格式有很多，包括m3u8、flv、mp4等。播放器有自己的一套逻辑去选择播放格式。对于H5来说，默认播放低清版本来节省流量，如果用户使用了切换清晰度的功能，那我们会默认打开他选择的版本。格式方面，则默认播放mp4，用户也可以设置qualitySort来优先播放高清的的版本。对于Flash来说，默认格式顺序是m3u8、flv、mp4。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 2]

# 二、功能介绍及启用 #

 *  创建播放器

1.  引用正确的JS和CSS文件
2.  添加播放器容器 需要设置容器的id属性，另外2.0.1之前的版本要添加prism-player类型。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 3]

 *  New Aliplayer创建播放

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 4]

 *  在线配置，用户可以预先体验下播放器的情况

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 5]

 *  Aliplayer-Cli创建演示例子

用户需要演示例子的时候，不需要写很多代码，通过这个命令，就可以创建例子，直接体验AliPlayer。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 6]

 *  PC端支持m3u8

播放域名启用允许跨域访问

 *  订阅和取消事件

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 7]

 *  清晰度切换

H5 1.9.9以后的版本和id+playauth播放方式才支持清晰度切换；支持记忆选择的清晰度，当选择的清晰度不能播放时，自动选择下一个清晰度播放。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 8]

 *  手动切换视频-H5

这个功能播放器内比较常见。我们把它分成两种情况去处理，如果是地址播放，我们通过loadByUrl来播放；如果是vid+playauth播放，我们通过replayByVidAndPlayAuth的方法来播放。

 *  手动切换视频-flash

地址播放方法与H5的方法一样，vid+playauth播放则需要先销毁播放器，再重新创建播放。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 9]

 *  不同地址格式的切换

只能先销毁播放器，再重新选择正确的播放器播放。Github地址看simple demo：https://github.com/alilmq/aliplayer-simple-demo

!\[b\_3\_7\]

 *  UI自定义

很多用户有这个需求，所以我们的UI是可以隐藏掉的。提供了一个skinLayout的属性，当这个属性没有指定值的时候，UI组件是全部显示。如果是空数组的时候，UI组件全部不显示。并且可以自定义组件的显示和位置，在默认UI基础上去裁剪，2.3.0版本以后，用户也可以通过自定义插件的方式自定义自己的UI。

 *  截屏

H5启用:

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 10]

FLASH启用：snapshot:true

H5播放器，播放域名需添加允许跨域访问的header

支持订阅snapshoted事件，获取截屏的时间点和数据：

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 11]

支持设置截图的大小和质量：

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 12]

支持添加文字水印：

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 13]

 *  边转变播功能

边转边播是MTS的功能，播放器可以支持这种场景的播放。第一次观看的时候调用MTS API启动转码，边转码边播放，而且可以设置延迟播放。转码中使用直播播放器，转码完成后使用点播方式播放。

 *  H5 android微信同层播放

因为H5在android端微信打开时，会自动全屏播放，覆盖Dom元素。

同层播放一般有两种业务场景，一种是点播的，视频在某个地方播放，下面的评论、播放列表等,demo地址：https://github.com/alilmq/h5demo

还要一种场景是直播场景，视频需要全屏。可以通过设置x5\_type:h5启用同层播放。Demo 地址：https://github.com/alilmq/h5livedemo

另外H5微信同层播放，有两篇文章可以参考:

http://player.alicdn.com/aliplayer/docs/blogs/how-to-handle-h5-same-layer.html

http://player.alicdn.com/aliplayer/docs/blogs/how-to-handle-h5-same-layer.html

 *  国际化

提供language属性，用于启用各种语言，默认为zh-cn，可选值为zh-cn or en-us。

 *  倍速播放

提供UI的版本，只提供了0.5、1、1.5、2四种倍速播放；而setspeed方法，可以随意设置倍速播放。这个可能会有一些限制，移动端有的浏览器会不支持，比如android微信。

 *  对于直播播放失败的处理

在播放失败时候，会尝试重新播放，触发onM3u8Retry事件，事件里可以做一些提示，比如主播离开请稍等；如果几次尝试后还是失败，会出发livestreamstop事件，事件里做一些直播失败或结束的提示。

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 14]

--------------------

# 三、其他辅助功能及工具 #

我们也做了一些辅助工具，方便用户去接入和排查问题。

 *  诊断工具

通过错误码描述的映射关系，大概能知道用户的错误所在；

通过vid知道用户播放的是哪个视频；

通过uuid这个唯一标识，可以在日志系统中查到用户的播放状态；

通过requestid和播放时间，可以定位到用户的错误是哪次播放的错误和具体的播放时间。

这里还有一个诊断的功能，可以知道用户环境的具体信息，省去手工获取视频的繁琐，可以快速诊断问题。

地址：http://player.alicdn.com/detection.html

![阿里云播放器SDK的正确打开方式：Aliplayer Web播放器功能实现][SDK_Aliplayer Web 15]

 *  检测工具

关于视频播放失败，我们提供了三种方式，原生H5、阿里云H5、阿里云Flash。我们把播放的日志调出来，通过日志来情况来判断播放失败的原因。举个例子，如果用户刚开始请求数据时就失败的话，那我们会猜测存在鉴权失败的情况；如果加载数据出错，那可能是用户的网络的原因；如果是开始播放后出错，可能就问题就出在解析或播放器不支持等方面。

 *  ffmpeg查看视频信息

有的用户只有画面，没有声音。我们可以通过ffmpeg可以看下视频的格式、流的情况、码率、帧率等。

最后，阿里云播放器的所有情况都聚合在以下的网站上：

http://player.alicdn.com/detection.html，其中包括帮助文档、在线配置、诊断工具、产品demo等，大家可以登录了解详情。


[SDK_Aliplayer Web]: /pro/os/crawler/J7ZA-73IM-YIN2.jpg
[SDK_Aliplayer Web 1]: /pro/os/crawler/VYRR-AVU2-U2AM.jpg
[SDK_Aliplayer Web 2]: /pro/os/crawler/NN3Y-VBMM-FYYB.jpg
[SDK_Aliplayer Web 3]: /pro/os/crawler/MFJU-UUJ3-YUJJ.jpg
[SDK_Aliplayer Web 4]: /pro/os/crawler/ZBFU-MJRI-BYI2.jpg
[SDK_Aliplayer Web 5]: /pro/os/crawler/YRRM-FR3Q-AAY2.jpg
[SDK_Aliplayer Web 6]: /pro/os/crawler/AYIV-7F3M-QVVJ.jpg
[SDK_Aliplayer Web 7]: /pro/os/crawler/Z3AE-YZ6R-YNJA.jpg
[SDK_Aliplayer Web 8]: /pro/os/crawler/M32Q-Z36R-EJZU.jpg
[SDK_Aliplayer Web 9]: /pro/os/crawler/M7JR-YY7B-QQBZ.jpg
[SDK_Aliplayer Web 10]: /pro/os/crawler/JB2Y-YYBA-QREQ.jpg
[SDK_Aliplayer Web 11]: /pro/os/crawler/JAJB-FENI-J6VU.jpg
[SDK_Aliplayer Web 12]: /pro/os/crawler/A2IZ-6BI7-ZQN2.jpg
[SDK_Aliplayer Web 13]: /pro/os/crawler/MFBA-AAQM-AAJA.jpg
[SDK_Aliplayer Web 14]: /pro/os/crawler/NVJY-REBU-MQUV.jpg
[SDK_Aliplayer Web 15]: /pro/os/crawler/E7ZR-VMJI-JFR3.jpg
 *  **原文作者：** 云栖社区
 *  **原文链接：** https://www.toutiao.com/item/6486341836599198221/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。