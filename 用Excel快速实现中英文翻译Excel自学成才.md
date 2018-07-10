---
title: 用Excel快速实现中英文翻译
date: 2018-06-29 23:49:10
categories: "开发"
tags:
	- 网易有道
	- Excel
	- 技术
	- JSON
	- 英语

---

# 近日，刷到一条抖音，看到用Excel进行中英文翻译，今天给大家讲解一种实现的方法。 #

在A列输入中文句子或英文句子，在B列便能得到翻译的结果，如下所示：

![用Excel快速实现中英文翻译][Excel]

在B列只需要输入一个公式，便可以得到结果：

在B2输入的公式为：

=TRIM(SUBSTITUTE(MID(SUBSTITUTE(WEBSERVICE("http://fanyi.youdao.com/translate?&i="&A2&"&doctype=json"),"""tgt"":""",REPT(" ",500)),500,500),"""\}\]\]\}",""))

有可能公式不会正常显示，下面将完整公式用图片格式再发一次：

![用Excel快速实现中英文翻译][Excel 1]

**公式解释：**

❶首先使用webservice函数嵌套使用，去有道翻译里面获取数据，WEBSERVICE("http://fanyi.youdao.com/translate?&i="&A2&"&doctype=json")

这部分公式（后面简称公式❶）得到的数据结果是：

\{"type":"ZH\_CN2EN","errorCode":0,"elapsedTime":0,"translateResult":\[\[\{"src":"你好啊","tgt":"How are you?"\}\]\]\}

很明显，我们想把"tgt":"后面的结果进行输出显示

❷所以使用SUBSTITUTE(公式❶,""**"tgt"":"**"",REPT(" ",500))，将"tgt":"替换成500个空格，所以得到的结果是：

\{"type":"ZH\_CN2EN","errorCode":0,"elapsedTime":0,"translateResult":\[\[\{"src":"你好啊",**500个空格**How are you?"\}\]\]\}

❸再使用Mid(公式❷,500,500)，从第500个字符开始取，那么前面所有的数据会自动被删除掉，然后再取500个数（或者更大都可以），这部分数据得到的结果是：

**少数空格**How are you?"\}\]\]\}

❹使用我们将"\}\]\]\}替换成空字符，也就是使用公式substitude(公式❸,"""\}\]\]\}","")，得到的结果是：

少数空格How are you?

❺最后使用trim函数将少数空格去除，trim(公式❹)，便得到了我们最终的结果

How are you?

上面是公式的理解过程，这套公式经常用来提取特定字符后面的数据，也是万金油公式之一吧，理解了对提取数据技巧上有所帮助，

如果没理解也没关系，我们直接套用公式使用

![用Excel快速实现中英文翻译][Excel]

你学会了吗？欢迎在下方留言讨论！

\--------------------------

欢迎关注，更多精彩内容持续更新中....


[Excel]: /pro/os/crawler/NBZ2-IENE-UQ6B.gif
[Excel 1]: /pro/os/crawler/AVUF-M23M-VYFF.jpg
 *  **原文作者：** Excel自学成才
 *  **原文链接：** https://www.toutiao.com/item/6572534125608043011/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。