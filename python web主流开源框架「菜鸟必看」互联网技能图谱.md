---
title: python web主流开源框架「菜鸟必看」
date: 2018-04-27 22:08:01
categories: "开发"
tags:
	- 机器学习
	- Django
	- 编程语言
	- Python
	- Flask

---

**1、flask**


![python web主流开源框架「菜鸟必看」][python web]

flask灵活，较轻，自由度高，适合深度学习python，对加深理解python有很大帮助。切记很多插件也有大坑。较bottle插件多，生态更好。


**2、django**

![python web主流开源框架「菜鸟必看」][python web 1]

django复杂，较重，功能丰富，提供整套解决方案，很多东西做了封装（比如models，users，authentication），自带的admin非常方便，适合快速开发，快速迭代项目。orm，modelform

**3、tornado**

![python web主流开源框架「菜鸟必看」][python web 2]

tornado使用了异步驱动，所以在写业务代码时如果稍有同步耗时性能就会急剧下降，加深对异步编程的理解。需要开发者自己扩展数据库操作。

BTW：知乎就是基础 Tornado 开发的。

**4、Bottle**

![python web主流开源框架「菜鸟必看」][python web 3]

Bottle 和 Flask 都属于轻量级的 Web 框架。但是 Bottle 似乎落寞了。我觉得跟他的 API 设计有关系。个人认为 Bottle 使用起来不那么顺手，因此也用得少。这里不做太多介绍。

**5、web.py**

![python web主流开源框架「菜鸟必看」][python web 4]

web.py也是很轻的一个框架，使用不多，也不做介绍。

**6、web2py** 

![python web主流开源框架「菜鸟必看」][python web 5]

web2py 是用于敏捷地开发安全的、数据库驱动的web应用；是一个full－stack框架，包含了开发完整功能的web应用所需的所有组件。

web2py是 Google 在 web.py 基础上二次开发而来的，兼容 GAE 。是为了安全而构建的。这意味着遵循成熟的方法，它能自动处理许多可能导致安全漏洞的问题。例如，web2py验证所有输入（防止注入攻击），转义所有输出（防止跨站点脚本攻击），重命名上传文件（防止目录遍历攻击）。在与安全有关的方面，web2py没有留给应用程序开发人员选择的余地。

**7、Quixote**

![python web主流开源框架「菜鸟必看」][python web 6]

著名的 豆瓣 就是基于 Quixote 开发的。跟上面几个框架不同，Quixote 的路由会有些特别。另外 Quixote 的性能据说也好。

**8、Pyramid**

![python web主流开源框架「菜鸟必看」][python web 7]

Pyramid更适合做一个想「长久」的应用。插件丰富且由官方支持。可扩展的模板。Pyramid以执行效率和快速开发的能力著称．这个框架最有优势的地方是，它包含了一些Python/Perl/Ruby独有的特性．这个开源框架拥有不依赖平台的MVC架构，和最快的启动开发的能力。


[python web]: /pro/os/crawler/EINJ-32IV-BUMM.jpg
[python web 1]: /pro/os/crawler/7ZAA-E23I-VJNU.jpg
[python web 2]: http://p3.pstatp.com/large/pgc-image/152483693702212d614de79
[python web 3]: http://p3.pstatp.com/large/pgc-image/152483694996439413579d7
[python web 4]: http://p3.pstatp.com/large/pgc-image/1524837409760ad501e447a
[python web 5]: http://p3.pstatp.com/large/pgc-image/1524836971837aab15cfa5f
[python web 6]: http://p1.pstatp.com/large/pgc-image/1524837668336675f39d7fe
[python web 7]: http://p1.pstatp.com/large/pgc-image/1524837010772909b2b7ebb
 *  **原文作者：** 互联网技能图谱
 *  **原文链接：** https://www.toutiao.com/item/6549129690034995725/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。