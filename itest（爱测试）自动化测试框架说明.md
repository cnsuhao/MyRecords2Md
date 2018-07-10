---
title: itest（爱测试）自动化测试框架说明
date: 2018-05-21 23:12:24
categories: "开发"
tags:
	- java
	- 测试框架
	- el表达式
	- AviatorEvaluator 解析器
	- Rest Assured 请求框架
	
---

![itest](/pro/os/test.jpg)
### 缘由

开发过程中,免不了做API测试,同时测试人员也避免不了来来回回、反反复复测试相同的功能（这里不考虑UI测试框架，因为我不熟悉，我单看了一些软件安装都够了）。以测试登陆为例，避免不了每次先登陆再测试，pc还好，如果是android、ios是不是很烦，如果是小程序模拟多个角色用户，你一个微信可咋办呢，开发环境还稍微好办，难道测试环境还要开发人员给你分配模拟？显然，这里的一切，我没找到合适的解决办法。
	后来想到API测试，通常来讲，开发人员做单元测试或者使用testNG集合reportNG就可以实现API测试了，但我想，你开发人员能一次测试模拟多个用户吗？能测试不同场景的组合吗？
	显然，在网络上搜索了众多测试框架后，我决定了，从简单出发，构建一个实用的测试框架。以下为做测试框架前，参考的资料：

*	测试框架部分
	*	[APIAutoTest框架](https://www.cnblogs.com/weientesting/p/7407425.html)
		* 	数据驱动设计，使用TestNG中的$DataProvider读取Excel中存储的自动化测试用例。
		*  基于TestNG测试框架
		*  使用HttpClient发送Http请求，并统一接口response返回值为String
		*  使用fastJson和Jsoup进行数据解析，由于请求返回值的统一，解析数据异常方便，方便接入不同接口类型的数据
		*  独立封装的检查点“Jsonpath”检查点，极大方便检查点的设置
		*  在线报告以及Email报告
		*  持续集成、持续交付、自动构建与测试
	* 	[API自动化测试框架](http://www.51testing.com/html/85/n-3686785.html)
		*  数据驱动
		*  简单流程覆盖，快速迭代
		*  组合Case不需要Coding
	*  [javaWeb--API 自动化测试框架](https://testerhome.com/topics/3572)
	*  [python实现的测试思路](https://testerhome.com/topics/7260)
	*  [TestFrameWork API接口测试框架](https://blog.csdn.net/wwg377655460/article/details/49978957)
	*  [接口自动化测试 --- Rest Assured](https://www.jianshu.com/p/47e5af367db1)
	*  [http(s)接口自动化测试框架](https://testerhome.com/topics/8055)
	*  [http接口测试—自动化测试框架设计](https://my.oschina.net/hellotest/blog/499719)
	*  [ITester接口测试框架](https://blog.csdn.net/tobetheender/article/details/53333059)
*	mock部分
	* [自动化数据模拟平台](http://baijiahao.baidu.com/s?id=1596395206616971243&wfr=spider&for=pc)
	* [随机模拟 java 数据插件](https://www.oschina.net/p/jmockdata)
	* [javamock生成对象](https://www.oschina.net/p/jmockdata)
	* [Mockito：一个强大的用于Java开发的模拟测试框架](https://blog.csdn.net/zhoudaxia/article/details/33056093)
	* [基于mock.js的更加丰富](https://github.com/Marak/faker.js)
	* [一个强大的测试数据生成器 （yod）](https://cnodejs.org/topic/553e06ba3575612520161a12)

在参考了如上（其他很多觉得不合适的，就没在此列出）信息后，考虑到当前项目进展情况和需求，只能动手实现了，目标实现：

*	第一步**实现excel配置化**，完成**基础的api接口参数配置**、**post/get请求**、**session/token验证**、**返回结果断言**。[点击查看配置](/pro/os/自动化测试框架.xlsx)
* 第二步实现**请求参数模拟数据生成**
* **测试报表**生成研究，已经实现，[点击查看结果](/pro/itest/6bff95cdd68cbb37d4e2d20338ab7793/dev)
* 实现 flow/inter 测试断言中逻辑跳转（分两阶段，第一阶段实现excel配置往下可跳转）
* 实现测试接口可独立添加,流程可自由配置，支持第二阶段的断言可跳（依然只往下,不回跳,容易导致死循环）
* 实现定时测试和自动发送相关报告(其中测试报告已实**现邮件发送**，且可**下载zip包**查看测试报告&接口详情)

### 进度说明

*	已经实现前三步，基本上测试报告可用和接口说明可用。**其中file上传测试逻辑暂时搁置**。
*  主要参考和借用了了**excel配置方式**、**JSON解析**、**Assured网络请求框架**、**Aviator解析器**、**mock.js和yod模拟数据**生成等思路和技术。
*  后面在考虑使用内存数据库，实现基础配置，这样可以支持团队内部使用；届时开源和提供网络接口服务。

### 扩展部分

-	本部分扩展了Aviator解析器，实现了如下函数：
	* $name()
	* $cname()
	* $age(0,120)
	* $color()
	* $date('yyyy-MM-dd')
	* $url()
	* $cparagraph()
	* $email()
	* $phone()
	* $tel()
	* $card()
	* $integer(0,100)
	* $double()
	* $long()
	* $qq()
	* $avatar
	* $vin()
	* ... 后续根据需求增加
-	测试部分

``` java
AviatorEvaluator.execute("$name()"));
AviatorEvaluator.execute("$cname()"));
AviatorEvaluator.execute("$age()"));
AviatorEvaluator.execute("$age(10,99)"));
AviatorEvaluator.execute("$color()"));
AviatorEvaluator.execute("$date.now('yyyy-MM-dd HH:mm:ss.SS')"));
AviatorEvaluator.execute("$qq() + '@qq.com'"));
AviatorEvaluator.execute("$phone()"));
AviatorEvaluator.execute("$url()"));
AviatorEvaluator.execute("$email()"));
AviatorEvaluator.execute("$cparagraph(30,100)"));
AviatorEvaluator.execute("$card()"));
AviatorEvaluator.execute("$tel()"));
AviatorEvaluator.execute("$tel('021')"));
AviatorEvaluator.execute("$tel('0371',8)"));
AviatorEvaluator.execute("$vin()"));
AviatorEvaluator.execute("$title()"));
AviatorEvaluator.execute("$avatar()"));
AviatorEvaluator.execute("$double(2.1,3.4,1)"));
AviatorEvaluator.execute("$long(2,4)"));
AviatorEvaluator.execute("$date.between('2018-01-01 12:01:10','2018-01-11 19:01:10','yyyy-MM-dd HH:mm:ss')"));
AviatorEvaluator.execute("$array.names(2)"));
``` 