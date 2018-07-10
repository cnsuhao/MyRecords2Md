---
title: api文档自动生成工具
date: 2018-04-17 14:20:03
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言
	- JSON
	- HTML

---

# api-doc #

java开发，根据代码自动生成api接口文档工具，支持RESTful风格

# 预览 #

![api文档自动生成工具][api]

基本信息

![api文档自动生成工具][api 1]

演示

![api文档自动生成工具][api 2]

数据模拟mock

在线预览地址

http://lovepeng.gitee.io/apidoc

# 开发原理 #

这个工具是一个典型的前后端分离开发的项目，想了解前后端分离开发的同学也可以下载本项目学习。

项目后端使用java代码，前端使用angular开发。Java开发时，使用注解把文档相关信息标注在类的方法上，通过工具自动扫描代码的注解，生成json数据，发给前端，前端angular解析生成页面

本项目自带一个spring-boot框架为基础的demo（这里使用spring-boot做演示的demo仅仅是为了方便，本质上只要是java写的项目都可以用该工具），前端用angular做了一个比较漂亮的界面（最终前端界面都编译成了html，如果你前端不熟悉，可以跳过，不用管他），这里使用angular开发仅仅是我比较喜欢，你可以用任何你喜欢的的前端框架或者仅仅使用html写一个漂亮的界面就可以。

后端项目开源地址：https://github.com/liepeng328/api-doc

前端开源地址：https://github.com/liepeng328/api-doc-angular

![api文档自动生成工具][api 3]

功能目录对应关系

![api文档自动生成工具][api 4]

请求参数和响应参数对应

# 快速启动 #

当成一个工具类用就可以了，下载本项目，拷贝包com.apidoc下的代码到你的系统，

然后拷贝前端html页面，在static.apidoc文件下，到你的资源文件下。即可使用

使用时，后台提供两个接口，目录文档接口和某个功能的详细接口

``````````
//生成目录接口ApiDoc apiDoc = new GeneratorApiDoc() .setInfo(//设置文档基本信息 new ApiDocInfo() .setTitle("某莫系统后台管理文档") .setVersion("1.0") .setDescription("") ) .generator(packageName);//指定生成哪个包下controller的文档System.err.println(JsonUtil.toString(detail));//详细功能接口ApiDocAction detail = new GeneratorApiDoc() //设置数据库连接信息，可忽略 .setDriver(driver) .setUrl(url) .setUserName(userName) .setPassword(password) .setDataBaseName(dataBaseName) .getApiOfMethod(methodUUID); System.err.println(JsonUtil.toString(detail));
``````````

# 一个详细的例子 #

一个详细例子如下代码，这里是springboot/springmvc的controller示例（展示两个文档，前端接口和后台接口）参考代码这个类 UserController.java

# 注解详细介绍 #

共有6个注解，标注出整个文档信息（我为什么讲那么详细，那么啰嗦，而且我没有把这个项目打成jar包直接给别人使用，就是因为文档生成最大可能是需要特殊定制，确保你拿到该代码可以个性化定制功能，随意修改）。

 *  Api 标注文档的功能模块
 *  ApiAction 标注一个功能
 *  ApiReqAparams 请求参数
 *  ApiResqAparams 响应参数
 *  ApiParam 参数，用以组成请求参数和响应参数
 *  Table 用以标注实体类（比如bean）和数据库表的关系，自动从数据库读取相关信息，不用写大量的 ApiReqAparams和ApiResqAparams

详细介绍如下

Api：写在类上，表明一个功能模块。

属性：

 *  name 模块名称
 *  mapping url映射
    
    ![api文档自动生成工具][api 5]

ApiAction： 写在方法上，表明一个功能点

属性：

 *  name 方法的功能名称
 *  mapping url映射
 *  description 描述
 *  method 请求方式（get，post，put，delete）
    
    ![api文档自动生成工具][api 6]

ApiReqParams： 请求参数

属性：

 *  type：参数类型

>  *  header 在请求头
>  *  url 在url后拼接
>  *  form 表单数据
>  *  json json格式

 *  ApiParam :参数列表

>  *  value : class类，增加该类可自动读取数据库信息，避免写多个属性
>  *  remove： 配合value使用，去除class类中无用的属性，比如id
>  *  dataType: 数据类型（字符串string,数字number,文件file,日期date,对象object,数组array,布尔类型boolean）
>  *  descrption:描述
>  *  defaultValue： 默认值
>  *  required：是否必须
>  *  object：从属于哪个对象（因为请求参数或者响应参数可能是对象中嵌套对象的，这里为了更好的表示这种层级关系，增加两个属性，object和belongTo，构建一个树结构，表示对象之间无限、互相嵌套）
>  *  belognTo ： 对应object 默认值为"0"，字符串0
>     
>     ![api文档自动生成工具][api 7]

ApiRespParams： 响应参数

属性：

 *  ApiParam： 该参数等同于请求参数中的ApiParam，参考如上描述
    
    ![api文档自动生成工具][api 8]

# 下载本项目并运行 #

配置jdk8以上版本，下载代码，运行ApidocApplication类main方法即可。

然后访问地址 http://localhost:8080/index.html

![api文档自动生成工具][api 9]

![api文档自动生成工具][api 10]

![api文档自动生成工具][api 11]

# 感谢列表 #

该项目为maven项目，引用工具请查看 pom.xml

感谢 spring-boot

感谢@路晓磊 的工具类hutool https://gitee.com/loolly/hutool

感谢阿里fastjson


[api]: /pro/os/crawler/36V3-2IJZ-NZ7N.jpg
[api 1]: http://p3.pstatp.com/large/pgc-image/152394594969157f89f50b2
[api 2]: http://p3.pstatp.com/large/pgc-image/1523945949801569fb3ca49
[api 3]: http://p1.pstatp.com/large/pgc-image/1523945949818f7c62375e8
[api 4]: http://p3.pstatp.com/large/pgc-image/152394594990265382aae28
[api 5]: http://p9.pstatp.com/large/pgc-image/152394594978890506c88cb
[api 6]: http://p9.pstatp.com/large/pgc-image/1523945949934ce6a40b490
[api 7]: http://p3.pstatp.com/large/pgc-image/1523945950064a47e8fa29a
[api 8]: http://p3.pstatp.com/large/pgc-image/15239459500552fa50f4968
[api 9]: http://p3.pstatp.com/large/pgc-image/1523945950121c1929186b1
[api 10]: http://p3.pstatp.com/large/pgc-image/1523945950088c540af7c4c
[api 11]: http://p3.pstatp.com/large/pgc-image/152394595015914d5640768
 *  **原文作者：** 大括号
 *  **原文链接：** https://www.toutiao.com/item/6545298243813638669/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。