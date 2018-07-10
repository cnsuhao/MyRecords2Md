---
title: 爬虫遇到图片禁止访问（如403）
date: 2016-07-08 16:35:16
categories: "开发"
tags:
	- 爬虫

---

今年一直在做爬虫，有时候碰到图片禁止访问的情况，以前一直以为不能解决。前两天在网上看了下资料。

对于低级的图片防盗链可以通过改变Referer来解决。

访问图片资源：

``````````
url = new URL(src);
                URLConnection con = url.openConnection();
                con.setConnectTimeout(5*1000);
                String referer = url.getProtocol()+"://"+url.getHost();
                con.setRequestProperty("Referer", referer);
``````````

在jsoup中也可以通过下面方式访问：

``````````
Jsoup.connect(url).ignoreContentType(true).referrer(referer).get()
``````````



 *  **原文作者：** lf_breeze
 *  **原文链接：** https://blog.csdn.net/lf_breeze/article/details/51862107
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。