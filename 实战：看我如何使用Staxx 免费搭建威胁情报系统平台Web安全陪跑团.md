---
title: 实战：看我如何使用Staxx 免费搭建威胁情报系统平台
date: 2018-06-04 14:03:38
categories: "开发"
tags:
	- 科技
	- Google
	- 软件

---

**本文主要是，来讲述STAXX的安装搭建过程，其它介绍大家可以百度or google，这里不在 浪费大家时间了（下面简要介绍，可以简单参考一下）**

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx]

> STAXX是Anomali公司提供的完全免费的威胁情报���统，威胁馈送领域里，STIX(结构化威胁信息表达)和TAXII(可信自动化指标信息交换)，是分析师以标准化方式获取情报的两项核心技术。
> 
> 企业消费STIX和TAXII的主要方式之一，就是通过存款信托及结算机构(DTCC)与金融服务信息共享和分析中心(FS-ISAC)联合开发的免费 Soltra Edge 软件。但该服务如今已被关停。
> 
> Anomali的STAXX是对Soltra关停的直接响应，旨在帮助企业能继续方便地从STIX和TAXII受益。

**下载过程**

如果大家要进行正常的下载，需要首先登录https://www.anomali.com/platform/staxx，然后点击download，接下来就是要求大家邮箱等信息注册，然后将下载连接发送到邮箱中，点击邮箱中的下载，才能进行，我这里就直接将我注册完获取的下载链接直接给到大家如下：https://anomali.cdn.rackfoundr ... 3.zip

（大概1.06G需要时间比较长，大家不要使用手机热点哟，土豪���随意）

【我下面的安转过程是在kali里面进行的】

**安装**

 *  第一步：解压缩下载下来之后，观察是一个zip的压缩文件，解压缩之后里面包含了三个文件：STAXX SLA.txt、Anomali\_STAXX\_Installation\_&\_Administration\_Guide.pdf、Anomali\_STAXX\_v3.4.0.ova

 *  第二步：打开VMware，导入ova文件

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 1]

> 下面就是下一步，下一步，不再啰嗦了

**初始化过程**

1.  登录系统修改密码

> 最后安装完了之后的界面如下（提示信息里包含了登录帐号和密码, 默认帐号：anomali 密码：anmalistaxx）
> 
> 初次登录系统，要求更改密码如下图

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 2]

> 这里会看到访问staxx的连接为https://x.x.x.x:8080

**开始配置STAXX**

浏览器地址栏输入前面得到的访问地址：https://172.16.155.155:8080/

得���下面的登录界面，界面上有提示web登录的帐号

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 3]

> STAXX LOGINFirst time login?
> 
> username: admin
> 
> password: changeme

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 4]

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 5]

恭喜你看到了希望，是不是很easy

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 6]

进入界面之后我们看到两个主要的菜单：DASHBOARDS 和ACTIVITY ，其中DASHBOARDS 提供了主要的各种直观的图标；ACTIVITY提供了按照特定条件进行筛选查询的功能。

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 7]

DASHBOARDS：仪表板提供了在指定时间段存储在Anomali StAXX上的所有可观测值的摘要信息。仪表板内的几个图形视图显示可观察的类型、严重性、来源��可观察到的）、可信度等。

其中我们按照其中SCAN\_IP为例打开看一下，具体操作如下：

点开对应的柱状图部分如下图：

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 8]

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 9]

此时直接连接到公网的Anomali 的网站上，需要你注册一个账户，**特别提醒：注册一定需要翻墙，只能自行解决了。。。，否则无法进行人机验证如下图：**

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 10]

注册成功之后（**特别提醒：注册的时候不能用免费的邮箱，比如不能用qq等邮箱**）

然后你再点击上面ip地址链接的时候就会出现下面 anomali，完整详细的分析信息：

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 11]

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 12]

ok，具体这些信息，能给你带来多大的价值，就要看你是用来做什么了，比如做反垃圾邮件，反病毒，追踪分析，仁者见仁智者见智

这里只是简单给大家一个示例，如果想了解详细的产品说明及操作，可以点击主界面上的帮助菜单，如下图：

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 13]

这里有完整的手册

![实战：看我如何使用Staxx 免费搭建威胁情报系统平台][Staxx 14]

至此简单的安装部署及使用情况已经完成，大家可以参考本篇文章快速了解此威胁情报系统，然后根据自己的需要得到自己想要的东东。

本文章为原创，请尊重作者，珍惜作者的劳动成果，转发请著名出处！

推荐：


[Staxx]: /pro/os/crawler/IAAR-VZAM-VBNI.jpg
[Staxx 1]: /pro/os/crawler/QU22-EBNZ-NM7F.jpg
[Staxx 2]: /pro/os/crawler/VMAU-RVJE-YNFM.jpg
[Staxx 3]: /pro/os/crawler/VNA7-NUER-6322.jpg
[Staxx 4]: /pro/os/crawler/VENY-QVRJ-2E2A.jpg
[Staxx 5]: /pro/os/crawler/B7NA-EAYR-NZFA.jpg
[Staxx 6]: /pro/os/crawler/2YQZ-ZQAU-MFYE.jpg
[Staxx 7]: /pro/os/crawler/BNJJ-MFBU-IQRI.jpg
[Staxx 8]: /pro/os/crawler/VBAN-7R2Y-63QJ.jpg
[Staxx 9]: /pro/os/crawler/FEFF-ENA3-A2AV.jpg
[Staxx 10]: /pro/os/crawler/MZBU-NIBQ-3IRJ.jpg
[Staxx 11]: /pro/os/crawler/3ABB-7ZEV-JZJF.jpg
[Staxx 12]: /pro/os/crawler/IAFV-NJYF-IA2Q.jpg
[Staxx 13]: /pro/os/crawler/RYAF-YFVF-Q2YN.jpg
[Staxx 14]: /pro/os/crawler/UMER-JNJQ-IVYA.jpg
 *  **原文作者：** Web安全陪跑团
 *  **原文链接：** https://www.toutiao.com/item/6563106104006410765/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。