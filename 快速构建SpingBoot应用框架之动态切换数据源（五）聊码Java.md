---
title: 快速构建SpingBoot应用框架之动态切换数据源（五）
date: 2018-06-28 17:03:59
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言

---

在实际的生产应用中，往往会出现一个系统需要连接多个数据源的问题，尤其最近非常流行动不动就来个读写分离的系统忽悠外行，装个高大上。下面我给大家介绍一种动态切换多数据源的方案，供大家参考学习，有不对的地方请大家指教，话不累述，开始：

首先、新建4个Java类和一个自定义注解：

Java类：

DynamicDataSource.java 获取数据源

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot]

DynamicDataSourceAspect.java 负责拦截需要切换数据源的类

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 1]

DynamicDataSourceRegister.java 动态数据源注册：

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 2]

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 3]

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 4]

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 5]

自定义注解：

TargetDataSource.java 负责指定数据源

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 6]

其次application.yml文件配置如下：

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 7]

然后新建自己的Mapper文件，名字随便取：

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 8]

运行结果请各位亲，自行运行这里就不做列子。

至此Mapper多数据源介绍完毕，有不懂的同学，欢迎留言.有问毕答！

![快速构建SpingBoot应用框架之动态切换数据源（五）][SpingBoot 9]


[SpingBoot]: /pro/os/crawler/3IFJ-QRNY-ZA7R.jpg
[SpingBoot 1]: /pro/os/crawler/NNZY-BJAR-RUMU.jpg
[SpingBoot 2]: /pro/os/crawler/YYVY-2A6F-MA3Y.jpg
[SpingBoot 3]: /pro/os/crawler/JBBB-BAFE-BJBM.jpg
[SpingBoot 4]: /pro/os/crawler/NMU3-Y3IF-7RYU.jpg
[SpingBoot 5]: /pro/os/crawler/URJ6-RNQZ-2EFB.jpg
[SpingBoot 6]: /pro/os/crawler/II7V-EUFA-FJQY.jpg
[SpingBoot 7]: /pro/os/crawler/IMF6-3QIN-R3IB.jpg
[SpingBoot 8]: /pro/os/crawler/EYN3-22EN-QYMN.jpg
[SpingBoot 9]: /pro/os/crawler/YJRR-NIAQ-V2YM.jpg
 *  **原文作者：** 聊码Java
 *  **原文链接：** https://www.toutiao.com/item/6572056572254487048/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。