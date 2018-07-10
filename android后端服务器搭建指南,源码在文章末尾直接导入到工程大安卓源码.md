---
title: android后端服务器搭建指南,源码在文章末尾直接导入到工程
date: 2018-03-21 15:34:37
categories: "开发"
tags:
	- Java
	- IntelliJ IDEA
	- JSP
	- 编程语言
	- JSON

---

后端（语言java，框架ssm （springmvc+mybatis），开发工具 intellij IDEA）

直接进入主题

打开intellij IDEA

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android]

点击create New project 选项

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 1]

按照步骤点击next

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 2]

继续点下一步

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 3]

这两部基本都默认，最后点击finish

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 4]

默认进来是长这个样子，现在进行一下相关配置，才能达到我们想要的springmvc

配置步骤

点击File---project structure---modules在main建立java目录，一次建立包名，包名下面建立controller,exception,interceptor,mapper,po,service这些目录

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 5]

目录建立好后开始配置mybatis，springmvc相关

pom.xml添加相关libs（spring框架，mybatis，log4j，JSP tag，文件上传，json 转换，分页插件）

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 6]

接下来就是一些重要配置，而且是必要配置

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 7]

由于时间问题就不一一说了，红色圈的就是springmvc必要配置，按照这个模板配置，方框圈就是mybatis必要配置，按照对应目录配置。

配置完后就可以写实现了

写个service接口，定义一个方法

``````````
//商品查询列表 List<HomeItems> findItemsList(HomeItems itemsQueryVo) throws Exception;
``````````

有了接口，就去写个实现类ItemsServiceImpl

``````````
@Autowired private HomeMapper itemsMapper; public List<HomeItems> findItemsList(HomeItems itemsQueryVo) throws Exception { return itemsMapper.findItemsList(itemsQueryVo); }
``````````

itemsMapper.findItemsList(itemsQueryVo)就是直接调用HomeMapper.xml

``````````
<!-- 商品列表查询 --> <!-- parameterType传入包装对象(包装了查询条件) resultType建议使用扩展对象 --> <select id="findItemsList" parameterType="com.dandroid.service.po.HomeItems" resultType="com.dandroid.service.po.HomeItems"> SELECT items.* FROM items <where> <include refid="query_items_where"></include> </where> </select>
``````````

实现已经写好了，那我们就开始写个控制器来调用了HomeController

由于我们要返回给前台，所以交互式就输出json给前台

``````````
responseJson方法
``````````

 *  忘了一步数据库的搭建和连接配置

我们通过navicat.exe这个工具打开mysql，输入密码建立连接

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 8]

输入密码后，建立一个库为dandroid

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 9]

建立一个表items，里面建立一些字段，注意这里的字段最好跟实体一模一样名称，在里面添加一些数据，完成以后再把db.properties里面配置一下就完成了

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 10]

ok，最后一步就是开始运行了，idea的运行需要这样配置

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 11]

点击进来后，点击左上角+号，找到tomcat server---选择local host

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 12]

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 13]

这三个红圈需要添加一下，第一个是自己叫个名字，第二个是切换过来，点击第三个加好，把.war加过来，点击右下角ok

最后点击运行，运行的结果

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 14]

完美？

no no no

还没给前台输出json了，我们找到spring 控制器HomeController，找到@RequestMapping("/getIndexImage") 看一下我们url路径是getIndexImage，参数是@RequestParam(value = "pageNo", required = true) Integer pageNo, @RequestParam(value = "pageSize",required = false) Integer pageSize

最后我们把url拼接出来就是：http://localhost:8080/getIndexImage?pageNo=2&pageSize=1

这下有结果给前台了，才算完美？

no no no

最后一步打包放在服务器才算完美

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 15]

点击build--选择build artifacts...

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 16]

选择dandroid\_service:war 点击build，当控制台出现Compilation completed successfully in 2s 77ms

就是编译成功，编译成功就会在

![android后端服务器搭建指南,源码在文章末尾直接导入到工程][android 17]

生成一个.war文件包，这个包就是上传到服务，供前端使用

源码地址：http://www.dandroid.cn/?p=3635


[android]: /pro/os/crawler/VEUU-RNRJ-2MAU.jpg
[android 1]: /pro/os/crawler/IYMR-EUYF-ERII.jpg
[android 2]: http://p3.pstatp.com/large/pgc-image/1521616907117821c0c680b
[android 3]: http://p3.pstatp.com/large/pgc-image/1521616909587e7c225de53
[android 4]: http://p3.pstatp.com/large/pgc-image/152161698150871f0aa1794
[android 5]: http://p1.pstatp.com/large/pgc-image/1521616993514c99cac0167
[android 6]: http://p3.pstatp.com/large/pgc-image/1521617002246901bc2ebdc
[android 7]: http://p3.pstatp.com/large/pgc-image/1521616909091173ab6ba4c
[android 8]: http://p3.pstatp.com/large/pgc-image/15216169072023c8b9c5a66
[android 9]: http://p3.pstatp.com/large/pgc-image/152161690886909cbfa95ae
[android 10]: http://p1.pstatp.com/large/pgc-image/1521617044155bd3774b96e
[android 11]: http://p9.pstatp.com/large/pgc-image/152161690789126ad6b5ae4
[android 12]: http://p3.pstatp.com/large/pgc-image/1521616908607d25160d96d
[android 13]: http://p1.pstatp.com/large/pgc-image/1521617057143e091c4de4f
[android 14]: http://p9.pstatp.com/large/pgc-image/1521617078936a593adc464
[android 15]: http://p3.pstatp.com/large/pgc-image/1521616908223322ba18136
[android 16]: http://p1.pstatp.com/large/pgc-image/15216169085191c82c29fa5
[android 17]: http://p3.pstatp.com/large/pgc-image/15216169083910681c3fe28
 *  **原文作者：** 大安卓源码
 *  **原文链接：** https://www.toutiao.com/item/6535298160640672270/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。