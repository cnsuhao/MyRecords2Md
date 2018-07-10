---
title: Elastic-Job(四)：Elastic-Job简单的例子
date: 2018-04-17 11:10:31
categories: "开发"
tags:
	- Tomcat
	- 技术

---

首先奉上代码地址：https://github.com/liuweigg/Davis

我们先让代码跑起来，来一个简单的例子。

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job]

# 引入jar包 #

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job 1]

# 代码开发 #

 *  配置文件：在/Davis/src/main/resources/目录���增加spring-job目录，并新建文件spring-job.xml。

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job 2]

 *  代码

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job 3]

 *  运行

放到Tomcat跑一下看看，当然zookeeper要先启动起来。

控制台会输出：

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job 4]

反正是跑起来了，虽然到现在看起来没什么卵用。具体的好处，要自己体会一下。

我们把这个工程部署到两个Tomcat试试，启动两个Tomcat，会发现。

Tomcat1的控制台输出：

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job 5]

Tomcat2的控制台输出：

![Elastic-Job(四)：Elastic-Job简单的例子][Elastic-Job_Elastic-Job 6]

好像体会到一点好处了。


[Elastic-Job_Elastic-Job]: /pro/os/crawler/BBNM-VN2M-RBEM.jpg
[Elastic-Job_Elastic-Job 1]: /pro/os/crawler/RVU2-QBYZ-NZ2U.jpg
[Elastic-Job_Elastic-Job 2]: http://p3.pstatp.com/large/pgc-image/15239343594496635deaaf5
[Elastic-Job_Elastic-Job 3]: http://p9.pstatp.com/large/pgc-image/1523934383439e39b5b73bd
[Elastic-Job_Elastic-Job 4]: /pro/os/crawler/JVEI-7FFN-NVAQ.jpg
[Elastic-Job_Elastic-Job 5]: /pro/os/crawler/JJUA-BAIM-ZVVR.jpg
[Elastic-Job_Elastic-Job 6]: /pro/os/crawler/3I6V-MUMV-UYVF.jpg
 *  **原文作者：** 会点代码的大叔
 *  **原文链接：** https://www.toutiao.com/item/6545249404104737293/
 *  **版权声明：** 本��客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。