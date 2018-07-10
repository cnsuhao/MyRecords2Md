---
title: 功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy
date: 2018-06-01 08:51:40
categories: "开发"
tags:
	- 网络爬虫
	- Scrapy
	- Django
	- Redis
	- Python

---

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy]

# **Gerapy 简介** #

Gerapy 是一款分布式爬虫管理框架，支持 Python 3，基于 Scrapy、Scrapyd、Scrapyd-Client、Scrapy-Redis、Scrapyd-API、Scrapy-Splash、Jinjia2、Django、Vue.js 开发，Gerapy 可以帮助我们：

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 1]

1.  更方便地控制爬虫运行
2.  更直观地查看爬虫状态
3.  更实时地查看爬取结果
4.  更简单地实现项目部署
5.  更统一地实现主机管理
6.  更轻松地编写爬虫代码

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 2]

# Gerapy的安装 #

安装非常简单，只需要运行 pip3 命令即可：

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 3]

安装完成之后我们就可以使用 gerapy 命令了，输入 gerapy 便可以获取它的基本使用方法：

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 4]

 *  初始化

接下来我们来开始使用 Gerapy，首先利用如下命令进行一下初始化，在任意路径下均可执行如下命令：

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 5]

执行完毕之后，本地便会生成一个名字为 gerapy 的文件夹，接着进入该文件夹，可以看到有一个 projects 文件夹，我们后面会用到。

紧接着执行数据库初始化命令：

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 6]

接着我们只需要再运行命令启动服务就好了：

![功能比Scrapy强大但使用却最方便的分布式爬虫管理框架——Gerapy][Scrapy_Gerapy 7]

这样我们就可以看到 Gerapy 已经在 8000 端口上运行了。


[Scrapy_Gerapy]: /pro/os/crawler/EZIU-IYN3-MRZU.jpg
[Scrapy_Gerapy 1]: /pro/os/crawler/J3IE-6ZZF-RYVJ.jpg
[Scrapy_Gerapy 2]: /pro/os/crawler/NUN3-MYRJ-ZRFB.jpg
[Scrapy_Gerapy 3]: /pro/os/crawler/N7JN-RMIB-ANRU.jpg
[Scrapy_Gerapy 4]: /pro/os/crawler/AY2Q-2MUI-QJJQ.jpg
[Scrapy_Gerapy 5]: /pro/os/crawler/BMRN-ZIUI-FF6F.jpg
[Scrapy_Gerapy 6]: /pro/os/crawler/MUY2-ABEU-ZQ3U.jpg
[Scrapy_Gerapy 7]: /pro/os/crawler/IMVU-BMIM-FAIN.jpg
 *  **原文作者：** 咱小二
 *  **原文链接：** https://www.toutiao.com/item/6561575262041932301/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。