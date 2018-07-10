---
title: 对一款不到2KB大小的JavaScript后门的深入分析
date: 2018-04-16 01:54:00
categories: "开发"
tags:
	- 脚本语言
	- JavaScript
	- 编程语言
	- PHP
	- HTML

---

在一台被入侵的服务器上，我们发现了一个攻击者遗留下来的脚本。该脚本是由JavaScript编写的，主要功能是作为Windows后门及C&C后端使用。在这里我首先要向大家说声抱歉，为了保护客户的隐私，在本文中我不会对一些细节做太多的探讨和描述。

该脚本的体积非常的小只有不到2KB，唯一能表明它的存在的是一个名为“wscript.exe”的运行进程，这是一个合法的Windows程序。脚本的主要部分包含一个无限循环的命令等待，在将查询字符串“reflow”传递给C&C 之后，它会休眠4个小时。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript]

C&C的回调如下所示：

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 1]

为了获取更多的信息，我开始在各种搜索引擎和VirusTotal中搜索相关的代码段，但令我失望的是我什么也没发现。因此，我决定使用Recorded Future来帮助我寻找。Recorded Future可以通过扫描并分析成千上万网站、博客、twitter帐户的信息来找到目前和未来人们、组织、活动和事件之间的关联性。

在返回结果中匹配了三个在2017年12月删除的匹配项。缓存的数据和链接回的源帮助我用C&C包恢复了压缩文件。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 2]

在软件包中有四个主要脚本（3个PHP和1个JavaScript文件）被复制到Web服务器。web服务器可能受到攻击者控制或受到其它手段的危害。其中的主要脚本index.php包含了一个SVG动画，当访问者碰巧访问该页面后，会看到如下画面。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 3]

该脚本显示，当“reflow”传递到页面时，恶意JavaScript文件（被重命名为一个PNG文件）的内容将被发送到受害者PC，并通过后门脚本进行评估。恶意脚本会通过WMI来获取系统信息，然后将该信息作为其身份验证方法的一部分发回。

在这里我们可以看到，该恶意脚本被无限循环运行，等待上传，下载和执行等命令。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 4]

“mAuth”函数会生成短随机字符串，并将它们与系统信息连接起来，并在Base64编码后的Cookie中将其传递给C&C。这些随机字符串很重要，因为它们被用作标记来识别包含在它们之间的指令。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 5]

数据通过AJAX回传给C&C。这里有一个名为“FillHeader”的函数用来填充HTTP头。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 6]

以下是当受害者PC检查时HTTP请求的样子：

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 7]

对cookie值执行Base64解码结果在第二行。在第二个符号显示系统信息后，重复字符串上的Base64解码。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 8]

其中的一个PHP脚本似乎是一个模板，被使用HTML代码修改以使页面看起来合法（例如，它包含实际网页的一部分）。该脚本被重命名并由index.php脚本引用。该脚本具有负责上传和下载文件以及创建活动日志的所有功能。日志文件包括受害者的IP地址，上传和下载的文件，会话信息等。

“Authentication”函数读取来自受害者的cookie值并解析出系统信息，以及定义用于创建日志文件名的变量。受害者的用户名和计算机名称为MD5哈希，并被作为日志文件名称的一部分使用。当受害者PC连接到C&C时，会在C&C服务器上创建三个文件：

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 9]

包中的最后一个PHP脚本用于与受害PC进行交互，并将命令发送给受害PC。请注意timezone和有趣的login方法。

![对一款不到2KB大小的JavaScript后门的深入分析][2KB_JavaScript 10]

可用的命令非常有限，但这已经足以让攻击者将更多更强大的工具上传到受害者的PC上，并获取更进一步的网络访问权限。最后，如果攻击者意识到他们即将被发现，他们可以使用此脚本中内置的另一组命令，来删除所有重要的日志文件。


[2KB_JavaScript]: http://p1.pstatp.com/large/pgc-image/152380767651412ac7eef95
[2KB_JavaScript 1]: http://p3.pstatp.com/large/pgc-image/1523807676559bb6ced2a62
[2KB_JavaScript 2]: http://p1.pstatp.com/large/pgc-image/1523807676533fb1932a23b
[2KB_JavaScript 3]: http://p3.pstatp.com/large/pgc-image/15238076765327f911a957b
[2KB_JavaScript 4]: http://p3.pstatp.com/large/pgc-image/15238076759295e82475c48
[2KB_JavaScript 5]: http://p1.pstatp.com/large/pgc-image/1523807676556d2a2acecda
[2KB_JavaScript 6]: http://p3.pstatp.com/large/pgc-image/1523807676558a6a10cf11f
[2KB_JavaScript 7]: http://p1.pstatp.com/large/pgc-image/1523807676583b9a46a5032
[2KB_JavaScript 8]: http://p9.pstatp.com/large/pgc-image/1523807675883f39c53c2b7
[2KB_JavaScript 9]: http://p3.pstatp.com/large/pgc-image/152380767608844633e3002
[2KB_JavaScript 10]: http://p1.pstatp.com/large/pgc-image/1523807676559ff7b455211
 *  **原文作者：** 网络安全晴雨表
 *  **原文链接：** https://www.toutiao.com/item/6544704361711272456/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。