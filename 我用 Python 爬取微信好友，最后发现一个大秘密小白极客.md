---
title: 我用 Python 爬取微信好友，最后发现一个大秘密
date: 2018-05-03 10:39:44
categories: "开发"
tags:
	- 移动互联网
	- 编程语言
	- 微信
	- JSON
	- Python

---

![我用 Python 爬取微信好友，最后发现一个大秘密][Python]

# 前言 #

你身处的环境是什么样，你就会成为什么样的人。现在人们日常生活基本上离不开微信，但微信不单单是一个即时通讯软件，微信更像是虚拟的现实世界。你所处的朋友圈是怎么样，慢慢你的思想也会变的怎么样。最近在学习 itchat,然后就写了一个爬虫，爬取了我所有的微信好友的数据。并对其中的一些数据进行分析，发现了一些很有趣的事。

# 微信好友爬虫 #

此次的爬虫程序用到的库有很多，其中爬取微信数据用到的事 itchat。需要你先去下安装。安装完成以后，你就可以通过 itchat.login() 这个函数登陆你自己的微信。它回弹出一个网页登陆的二维码，你用手机扫描登陆即可。

然后通过 itchat.get\_friends() 这个函数就可以获取到自己好友的相关信息，这些信息是一个 json 数据返回。然后我们就可以根据这些返回的信息，进行正则匹配抓取我们想要的信息，在进行分析。

``````````
import itchatitchat.login()#爬取自己好友相关信息， 返回一个json文件friends = itchat.get_friends(update=True)[0:]
``````````

# 我的微信好友的男女比例 #

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 1]

观察返回的数据，很容易就可以根据关键字发现性别是存放在一个字典里面，它的 key 是「Sex」，男性值为 1，女性为 2，其他是不明性别的（就是没有填的）。

在代码里我定义了一个函数 parse\_friends() 通过一个 for 循环，把获取到的数据通过 for 循环保存到 text 字典里。然后再通过 draw() 函数画出柱状图。柱状图使用的是 plt 库，之前也写过一篇文章，感兴趣的同学可以去查看。

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 2]

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 3]

最后打印的结果：

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 4]

不得不多说我微信的 1K 多的好友男女比列非常的不协调，男多女少啊。这让我回想起以前高中一个班 50 个人，女生就 7 个，然后我们班的女生从此就有一个女团称呼「七仙女」。

# 我的微信好友个性签名的自定义词云图 #

为了进一步分析我的好友大致都有什么特征，我把好友的个性签名一起抓取，分析制作成词云。

个性签名是保存在 Signature 这个 key 中，由于有些签名包含些表情，最初抓取会变成 emoji、span、class 等等这些无关的词。所有需要先替换掉，另外，还有类似 <>/= 之类的符号，也需要写个简单的正则替换掉，再把所有拼起来，得到 text 字串。

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 5]

得到的数据最后保存到当前目录名为「text.txt」文本中。

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 6]

分析好友签名的函数我定义成:parse\_signature()，完整代码如下：

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 7]

抓取整理了签名的数据，接下来就是制作出词云。这里使用的是 wordCloud 来进行词云的制作。之前的文章也有介绍过词云的制作，感兴趣的同学可以查看这篇文章。

词云的制作我定义了一个:draw\_signature() 函数，完整代码如下

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 8]

运行上面的代码后得到了如下的图，由于好友数量比较多，我分别找了两张图制作出图云。

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 9]

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 10]

努力，奋斗，世界，生活，自己。这些词在我们 1K 多人的好友中出现的最多。大家都非常的优秀，都非常的上进。

![我用 Python 爬取微信好友，最后发现一个大秘密][Python 11]

我的签名：人生必有痴，而有后成。现在的我痴迷于各种优秀的人，每天都在向他们学习。希望大家一生当中也有痴迷的一面。

需要完整的代码后台私信「**代码**」，我发给你。


[Python]: /pro/os/crawler/VAY6-BZ7B-J2QJ.jpg
[Python 1]: /pro/os/crawler/F6FN-FE3I-EEFQ.jpg
[Python 2]: /pro/os/crawler/AZJV-32NQ-3Q7V.jpg
[Python 3]: /pro/os/crawler/6FVF-YUZV-UAUB.jpg
[Python 4]: /pro/os/crawler/AF7Z-VZIR-NERM.jpg
[Python 5]: /pro/os/crawler/YFEQ-6NVA-Q3EE.jpg
[Python 6]: http://p3.pstatp.com/large/pgc-image/1525314990142e62c710a3b
[Python 7]: /pro/os/crawler/YQYE-NUEU-YN3Y.jpg
[Python 8]: /pro/os/crawler/77FJ-NRQZ-IY2I.jpg
[Python 9]: /pro/os/crawler/FNYA-UQRA-NVYQ.jpg
[Python 10]: /pro/os/crawler/AII3-QABN-Q7NF.jpg
[Python 11]: /pro/os/crawler/ZE6B-JEUZ-ZIVM.jpg
 *  **原文作者：** 小白极客
 *  **原文链接：** https://www.toutiao.com/item/6551178834157240845/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。