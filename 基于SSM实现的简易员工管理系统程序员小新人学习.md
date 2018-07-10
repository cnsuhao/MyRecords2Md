---
title: 基于SSM实现的简易员工管理系统
date: 2018-06-29 17:22:17
categories: "开发"
tags:
	- Tomcat
	- 技术
	- Eclipse
	- XML
	- JSON

---

首先，页面的UI是使用了Bootstrap框架快速搭建的，这个框架还是比较好用的，不但快速，而且美观，风格偏扁平化。而且对于我这种英文渣渣来说，有中文的帮助文档，简直不要太好上手，然后搭建好的大致效果图就如1-1所示，当然搭建好的只是静态页面，下面的数据，按钮的button\_click事件都是后面自行编写的。

![基于SSM实现的简易员工管理系统][SSM]

图1-1

然后就要介绍介绍SSM框架的具体流程了，因为是在本地访问，没有放到联网服务器上，所以使用了Tomcat作为服务器，项目前端发起请求，发送到SpringMVC前端控制器中，再由SpringMVC前端控制器判断，是否能进行处理，能处理的，再发送给Controller，不能处理如静态页面之类的，直接发送给Tomcat服务器，让服务器进行解析。发送到Controller的数据，再调用Service层的业务逻辑。假如要进行数据库层的交互，就将其交给Dao层的组件，而Dao层的组件都是用MyBatis来写的，MyBatis的某某Mapper再进行与数据库的交互，同时，这些Mapper的文件和接口都是通过MyBatis Generator(MBG)自动生成的，但由于查询的时候还有多表联合查询，所以还在xml文件中，新写了两个方法，用来实现多表联合查询。同时，基本上的增删改查都是通过Ajax实现的，由Ajax发送请求，再返回Json，使用JS解析Json并在页面中显示。

项目是使用Maven进行依赖管理的，简单来说，就是通过Maven去下载项目所需的jar包，同时在项目完成后，可以用Maven构建war包，使项目部署在真正的服务器而不是Eclipse中的镜像服务器。

大体构建说完了，来说说项目中实际运用的注意点吧，在添加新员工的时候进行了前端、后端都校验的方法，可以极大程度的避免脏数据的添加，同时用了一些正则表达式来判断姓名和邮箱是否合法。前端发送的Ajax请求，要传递到后台处理，都只需要在方法上添加@ResponseBody和@RequestMapping("路径尾缀")即可。

最后，实现的图例演示。

员工添加功能的实现：

![基于SSM实现的简易员工管理系统][SSM 1]

![基于SSM实现的简易员工管理系统][SSM 2]

![基于SSM实现的简易员工管理系统][SSM 3]

修改功能的实现：

![基于SSM实现的简易员工管理系统][SSM 4]

![基于SSM实现的简易员工管理系统][SSM 5]

单个删除的实现：

![基于SSM实现的简易员工管理系统][SSM 6]

![基于SSM实现的简易员工管理系统][SSM 7]

批量删除：

![基于SSM实现的简易员工管理系统][SSM 8]

![基于SSM实现的简易员工管理系统][SSM 9]

这个基于SSM的基础员工管理系统，大致功能都如上图所示了，实现了基本的增删改查，下一步，如果继续完善，则添加上登陆页，再将项目发布到互联网的服务器上，没有登陆页的弊端太多了，就不论述了，再加上现在的都是测试数据，没有实际意义，所以暂不考虑发布到互联网。


[SSM]: /pro/os/crawler/QRYQ-6VZB-NNBI.jpg
[SSM 1]: /pro/os/crawler/EMYB-Z3FE-A7VY.jpg
[SSM 2]: /pro/os/crawler/QYQU-3AA7-ZBFV.jpg
[SSM 3]: /pro/os/crawler/VQMN-U2UM-FZIR.jpg
[SSM 4]: /pro/os/crawler/JAA6-7ZNA-RAYE.jpg
[SSM 5]: /pro/os/crawler/67BZ-Q3QF-Z7FV.jpg
[SSM 6]: /pro/os/crawler/B7BN-QIAJ-IUUA.jpg
[SSM 7]: /pro/os/crawler/UZNI-FIER-EN6N.jpg
[SSM 8]: /pro/os/crawler/RUUY-UAZ6-RYRZ.jpg
[SSM 9]: /pro/os/crawler/ZMYR-UFJB-FUAI.jpg
 *  **原文作者：** 程序员小新人学习
 *  **原文链接：** https://www.toutiao.com/item/6572434426075021832/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。