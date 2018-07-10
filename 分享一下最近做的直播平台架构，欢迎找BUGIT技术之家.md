---
title: 分享一下最近做的直播平台架构，欢迎找BUG
date: 2017-10-30 08:25:12
categories: "开发"
tags:
	- Nginx
	- 技术

---

**这是个无spring、无servlet、无mybatis的架构，用了国内优秀的jfinal, hutool, fastjson, druid等框架和工具，当然也用到了自家产的tio-core, tio-websocket, tio-httpserver，应该是一种极少见到的架构形态吧？**

直接上图是不是就可以了？欢迎各位提BUG，共同提高！

![分享一下最近做的直播平台架构，欢迎找BUG][BUG]

![分享一下最近做的直播平台架构，欢迎找BUG][BUG 1]

nginx部分配置。生产环境必然会用到upStream，这里只贴了开发环境的配置

![分享一下最近做的直播平台架构，欢迎找BUG][BUG 2]

私有协议的一部分内容，还会持续调整，此处仅供参考

![分享一下最近做的直播平台架构，欢迎找BUG][BUG 3]

进入直播间的过程

![分享一下最近做的直播平台架构，欢迎找BUG][BUG 4]


[BUG]: /pro/os/crawler/NFYI-AFV3-AAUN.jpg
[BUG 1]: /pro/os/crawler/ZV6J-INYV-ZE6R.jpg
[BUG 2]: /pro/os/crawler/UZBB-IBBE-FIB3.jpg
[BUG 3]: /pro/os/crawler/UFIF-RF22-M2QV.jpg
[BUG 4]: /pro/os/crawler/JZBF-QM6N-AU7R.jpg
 *  **原文作者：** IT技术之家
 *  **原文链接：** https://www.toutiao.com/item/6482308927122833934/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。