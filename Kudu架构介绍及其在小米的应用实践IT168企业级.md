---
title: Kudu架构介绍及其在小米的应用实践
date: 2017-06-19 20:02:43
categories: "开发"
tags:
	- 科技
	- Hive
	- Cloudera
	- HDFS
	- HBase

---

Kudu是Cloudera在15年9月开源的分布式数据存储引擎，其结合了Hbase和HDFS的优势，可以同时提供高效的随机访问以及数据扫描能力。Kudu支持数据的实时插入和分析，为实时的OLAP计算提供了另外一种选择。小米是Kudu在中国最早的一批用户，目前内部已经有较大规模的业务在使用，并且在不断探索新的应用场景。本次演讲将会介绍Kudu的大致技术架构，新版本的新增功能，以及未来的开发计划。同时会介绍Kudu在小米计算架构中所扮演的角色，分享一些Kudu在实际使用中的经验，希望可以促进Kudu在中国的发展和使用。

张震，曾就职于曾就职于老牌BI厂商MicroStrategy，15年加入小米云平台计算组，先后负责Impala，Hive，Kudu的维护和及内部需求开发。在分布式计算和存储领域有多年的积累和实战经验。

![Kudu架构介绍及其在小米的应用实践][Kudu]

![Kudu架构介绍及其在小米的应用实践][Kudu 1]

![Kudu架构介绍及其在小米的应用实践][Kudu 2]

![Kudu架构介绍及其在小米的应用实践][Kudu 3]

![Kudu架构介绍及其在小米的应用实践][Kudu 4]

![Kudu架构介绍及其在小米的应用实践][Kudu 5]

![Kudu架构介绍及其在小米的应用实践][Kudu 6]

![Kudu架构介绍及其在小米的应用实践][Kudu 7]

![Kudu架构介绍及其在小米的应用实践][Kudu 8]

![Kudu架构介绍及其在小米的应用实践][Kudu 9]

![Kudu架构介绍及其在小米的应用实践][Kudu 10]

![Kudu架构介绍及其在小米的应用实践][Kudu 11]

![Kudu架构介绍及其在小米的应用实践][Kudu 12]

![Kudu架构介绍及其在小米的应用实践][Kudu 13]

![Kudu架构介绍及其在小米的应用实践][Kudu 14]

![Kudu架构介绍及其在小米的应用实践][Kudu 15]

![Kudu架构介绍及其在小米的应用实践][Kudu 16]

![Kudu架构介绍及其在小米的应用实践][Kudu 17]

![Kudu架构介绍及其在小米的应用实践][Kudu 18]

![Kudu架构介绍及其在小米的应用实践][Kudu 19]

![Kudu架构介绍及其在小米的应用实践][Kudu 20]

![Kudu架构介绍及其在小米的应用实践][Kudu 21]

![Kudu架构介绍及其在小米的应用实践][Kudu 22]

![Kudu架构介绍及其在小米的应用实践][Kudu 23]

![Kudu架构介绍及其在小米的应用实践][Kudu 24]

![Kudu架构介绍及其在小米的应用实践][Kudu 25]

![Kudu架构介绍及其在小米的应用实践][Kudu 26]

![Kudu架构介绍及其在小米的应用实践][Kudu 27]

![Kudu架构介绍及其在小米的应用实践][Kudu 28]

![Kudu架构介绍及其在小米的应用实践][Kudu 29]


[Kudu]: /pro/os/crawler/I2M2-YNNE-Z7ZU.jpg
[Kudu 1]: /pro/os/crawler/ZFQV-ZMVN-J2EU.jpg
[Kudu 2]: /pro/os/crawler/VMVU-EUBE-ZFMY.jpg
[Kudu 3]: /pro/os/crawler/6BZJ-ERQY-QMIN.jpg
[Kudu 4]: /pro/os/crawler/MBAV-FQA6-FFNB.jpg
[Kudu 5]: /pro/os/crawler/NYJR-3U6V-IEUI.jpg
[Kudu 6]: /pro/os/crawler/VN2Y-7RQV-YV3I.jpg
[Kudu 7]: /pro/os/crawler/7F6N-MNNN-REJN.jpg
[Kudu 8]: /pro/os/crawler/MI7N-ABUZ-UUVE.jpg
[Kudu 9]: /pro/os/crawler/N2IU-VEUI-BVNQ.jpg
[Kudu 10]: /pro/os/crawler/BZBF-VVMZ-3YMZ.jpg
[Kudu 11]: /pro/os/crawler/ZRMB-7ZBF-UAYU.jpg
[Kudu 12]: /pro/os/crawler/ZVFV-JBYA-EB2Y.jpg
[Kudu 13]: /pro/os/crawler/IMBB-RERQ-VB6V.jpg
[Kudu 14]: /pro/os/crawler/NMZM-VZEB-IIUJ.jpg
[Kudu 15]: /pro/os/crawler/F2QU-BA3M-BMNR.jpg
[Kudu 16]: /pro/os/crawler/RYQY-FIIJ-7NME.jpg
[Kudu 17]: /pro/os/crawler/B3MJ-BMN7-7RA2.jpg
[Kudu 18]: /pro/os/crawler/EAI3-IV3Q-UIR2.jpg
[Kudu 19]: /pro/os/crawler/JIR2-YZM3-MEEB.jpg
[Kudu 20]: /pro/os/crawler/IJUV-3QZN-UYFZ.jpg
[Kudu 21]: /pro/os/crawler/R7F6-ZU7B-FE6Z.jpg
[Kudu 22]: /pro/os/crawler/Z2EN-VQAA-VIVU.jpg
[Kudu 23]: /pro/os/crawler/UEYF-NFFR-BRIR.jpg
[Kudu 24]: /pro/os/crawler/JYFB-MYUV-IRFQ.jpg
[Kudu 25]: /pro/os/crawler/7FN2-QRQU-NN2U.jpg
[Kudu 26]: /pro/os/crawler/ZNIB-FA63-QJVQ.jpg
[Kudu 27]: /pro/os/crawler/JBI2-MZJA-JFAQ.jpg
[Kudu 28]: /pro/os/crawler/YBZM-VYFA-I3YR.jpg
[Kudu 29]: /pro/os/crawler/BI6Z-JMYZ-MUEF.jpg
 *  **原文作者：** IT168企业级
 *  **原文链接：** https://www.toutiao.com/item/6433318823570440706/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。