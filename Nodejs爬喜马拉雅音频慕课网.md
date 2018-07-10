---
title: Nodejs爬喜马拉雅音频
date: 2018-07-01 14:42:54
categories: "开发"
tags:
	- 网络爬虫
	- GitHub
	- Node.js
	- JSON
	- 喜马拉雅山

---

最近忙着组NAS，一直没有时间更新。昨天忘了从哪理翻出了喜马拉雅的API，顺藤摸瓜就花了一个小时写了这个爬虫。大概你们觉得听喜马拉雅直接开web或者app就能满足了呀，合并爬硬盘到本地呢？如果我说，喜马拉雅会下架一些我喜欢的音频，然后再也听不到了呢？放在自己的盘里，虽然保不准哪天盘烧了，安全性不高，但是至少在盘没烧之前，控制权在自己手上呀。

**第一步：找API**

本来我是想找人人影视的api的，想搭好NAS就下载订阅的美剧，发现加密方式没弄懂，哪位小伙伴知道的麻烦教教我哈。所以想着要不先爬取喜马拉雅的音频吧，反正无聊的时候用蓝牙音箱听听也不错。搜了github找到一些爬虫，分析代码找到一个音频真实地址的API。但是这远远不够啊，我要下载的是整个专辑。

在检查面板里翻了一圈，在很多个API里面找到了两个适合的API。

![Nodejs爬喜马拉雅音频][Nodejs]

API中我使用了Lodash模板格式替换了参数，方便传参。

于是开始有了一个酱紫的思路。

找到想要的专辑->获取整个专辑下的所有的音频的ID->获取每一个音频ID的真实链接->保存成aria2批量下载文件->使用aria2下载

**根据思路选工具**

有了思路，还需要合适的工具，node有一些封装好的模块方便我们使用，当实在找不到合适的模块的时候，我们还能自己写模块。

因为这个爬虫不需要解析HTML，服务器返回的数据是json，所以我们只需要两个基础模块：request和lodash。

然而request是callback回调，然而我想用Promise方式，自己封装一个。

![Nodejs爬喜马拉雅音频][Nodejs 1]

emmmm......我在promise里面加了个callback，用于返回下载进度，方便以后接入进度条什么的。

哦，差点忘了，我们还需要安装aria2，linux的用户包管理安装，win用户自行下载exe文件后配置环境变量。要是实在不会，百度一下你知道。

这样工具都准备好了，接下来就是写代码的时间了。

**写代码**

先把封装好的模块和可能需要用的模块统一写在一个文件中，方便管理。

![Nodejs爬喜马拉雅音频][Nodejs 2]

**获取专辑下所有的音频**

![Nodejs爬喜马拉雅音频][Nodejs 3]

其中cfg这个是之前的API配置文件因为API每次返回的数据默认最大30条，所以直接在代码里写30了，这里返回专辑所以的音频的url，url里面有我们想要的ID。

**获取所以的音频的真实链接**

![Nodejs爬喜马拉雅音频][Nodejs 4]

返回的是一个字符串，按aria2批量下载文件的格式拼接的，最后要保存到文件中供aria2使用。

**最后导出模块**

![Nodejs爬喜马拉雅音频][Nodejs 5]

完整的代码如下：

![Nodejs爬喜马拉雅音频][Nodejs 6]

整个目录结构

![Nodejs爬喜马拉雅音频][Nodejs 7]

测试例子就不写了，emmmm.......我就是这么懒，来打我呀hiahiahiahiahia

> 本文原创发布于慕课网手记，作者：布宝，点击下方“了解更多”即可查看原文，转载请注明出处，谢谢合作！


[Nodejs]: /pro/os/crawler/RRF7-3ERJ-FIEI.jpg
[Nodejs 1]: /pro/os/crawler/ZBEF-JM2I-RNUV.jpg
[Nodejs 2]: /pro/os/crawler/JBV6-ZNFU-FEBJ.jpg
[Nodejs 3]: /pro/os/crawler/UNZB-NMM7-BEFN.jpg
[Nodejs 4]: /pro/os/crawler/EMYR-3AMV-J2EN.jpg
[Nodejs 5]: /pro/os/crawler/IFYY-BAFN-M77B.jpg
[Nodejs 6]: /pro/os/crawler/JJIU-ZA6Z-7BJU.jpg
[Nodejs 7]: /pro/os/crawler/AAIR-BZFF-MNYE.jpg
 *  **原文作者：** 慕课网
 *  **原文链接：** https://www.toutiao.com/item/6573135522091237891/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。