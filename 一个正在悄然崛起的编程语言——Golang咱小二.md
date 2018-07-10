---
title: 一个正在悄然崛起的编程语言——Golang
date: 2018-05-15 13:56:19
categories: "开发"
tags:
	- Google
	- Docker
	- Hadoop
	- 编程语言
	- Go语言

---

![一个正在悄然崛起的编程语言——Golang][Golang]

Go语言是谷歌推出的一种全新的编程语言，可以在不损失应用程序性能的情况下降低代码的复杂性。谷歌首席软件工程师罗布派克(Rob Pike)说:我们之所以开发Go，是因为过去10多年间软件开发的难度令人沮丧。

![一个正在悄然崛起的编程语言——Golang][Golang 1]

该语言自推出以来，Google的Go编程语言（Golang）越来越受主流用户的欢迎。在2016年12月的一份调研中，3,595名受访者中有89％表明他们在工作中或工作以外用Go语言编程；此外，在编程语言中，Go语言在专业知识和偏好方面排名最高。2017年7月，在Tiobe的年度编程语言排名中，Go语言从去年的第55名一跃跳到了第10名。

![一个正在悄然崛起的编程语言——Golang][Golang 2]

# 流行的go应用 #

 *  Go语言的杀手级应用就是Docker，Docker应该大家都知道，目前在国内火的一塌糊涂
 *  Codis，一种Redis的集权解决管理方案，很大部分go开发，由豆瓣推出。
 *  Glow，类似Hadoop，也是一种大数据处理框架，性能非常好，是Hadoop的go的实现。
 *  Cockroach数据库，译作蟑螂，意味着该数据库的生存能力很强，是高稳定性业务环境的首选数据库之一

![一个正在悄然崛起的编程语言——Golang][Golang 3]

# 使用golang语言读取文件 #

引入io/ioutil包，该包默认拥有以下函数供用户调用：

![一个正在悄然崛起的编程语言——Golang][Golang 4]

读取文件需要注意以下三个函数：


![一个正在悄然崛起的编程语言——Golang][Golang 5]

读取文件示例：

![一个正在悄然崛起的编程语言——Golang][Golang 6]

# 使用golang语言实现一个小顶堆 #

定义一个worker结构体， worker对象中存放很多待处理的request，pinding代表待处理的request数量，以worker为元素，实现一个小顶堆，每次Pop操作都返回负载最低的一个worker。

![一个正在悄然崛起的编程语言——Golang][Golang 7]

golang标准库中提供了heap结构的容器，所以只需要实现几个方法，就能实现一个堆类型的数据结构，使用时只需要调用标准库中提供的Init初始化接口、Pop接口、Push接口，就可以得到我们想要的结果。


[Golang]: /pro/os/crawler/EQYI-BEVU-R7BV.jpg
[Golang 1]: /pro/os/crawler/A2QF-U3II-UJJV.jpg
[Golang 2]: /pro/os/crawler/Q2MR-FVVB-Q6RF.jpg
[Golang 3]: /pro/os/crawler/JFVY-3YJM-IBIV.jpg
[Golang 4]: /pro/os/crawler/QJJF-MRYI-NQJ3.jpg
[Golang 5]: /pro/os/crawler/N6BJ-BNVB-YF6V.jpg
[Golang 6]: /pro/os/crawler/BRMR-RIAR-YM2E.jpg
[Golang 7]: /pro/os/crawler/UYUQ-QZZU-AJBY.jpg
 *  **原文作者：** 咱小二
 *  **原文链接：** https://www.toutiao.com/item/6555682514009063943/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。