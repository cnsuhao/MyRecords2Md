---
title: 代码质量管理的开源平台Sonar
date: 2018-05-08 09:08:01
categories: "开发"
tags:
	- Java
	- 脚本语言
	- Linux
	- Eclipse
	- MySQL

---

> **转发是对小编的最大支持**
> 
> **百度搜索：小强测试品牌**

**介绍**

Sonar是一个用于代码质量管理的开源平台，用于管理Java源代码的质量。通过插件机制，Sonar 可以集成不同的测试工具，代码分析工具，以及持续集成工具，比如pmd-cpd、checkstyle、findbugs、Jenkins。通过不同的插件对这些结果进行再加工处理，通过量化的方式度量代码质量的变化，从而可以方便地对不同规模和种类的工程进行代码质量管理。同时 Sonar 还对大量的持续集成工具提供了接口支持，可以很方便地在持续集成中使用 Sonar。此外，Sonar 的插件还可以对 Java 以外的其他编程语言提供支持，对国际化以及报告文档化也有良好的支持。

**SONAR安装&运行**

下载地址：http://www.sonarqube.org/downloads/

运行：解压后，根据平台运行bin下不同目录下的启动脚本。对于linux x86\_64，运行bin/linux-x86-64/sonar.sh。

可用命令：

./sonar.sh \{ console | start | stop | restart | status | dump \}

安装插件：

SONAR中文包：http://docs.codehaus.org/display/SONAR/Chinese+Pack

将插件放置在$\{SONARHOME\}/extensions/plugins下，重启sonar后生效。注意版本号要匹配。本例中，SonarQube版本为4.4，所以选择插件版本为1.8的。

![代码质量管理的开源平台Sonar][Sonar]

**SONAR + Maven分析代码质量**

**1）设置sonar使用的数据库信息。**

本例设置sonar使用mysql数据库存储分析数据。保存设置后，执行restart使其生效。

$\{SONARHOME\}/conf/sonar.properties:

``````````
# Permissions to create tables, indices and triggers must be granted to JDBC user.# The schema must be created first.sonar.jdbc.username=rootsonar.jdbc.password=root# Comment the following line to deactivate the default embedded database.#sonar.jdbc.url=jdbc:h2:tcp://localhost:9092/sonar#----- MySQL 5.x# Comment the embedded database and uncomment the following line to use MySQLsonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true
``````````

**2）需要在Maven的settings.xml设置sonar信息。**

其中<sonar.host.url>http://localhost:9000</sonar.host.url>指明了sonar服务器的地址。所以在执行maven命令的时候，<sonar.host.url>指明的服务器必须已运行起来。

$\{MAVEN\_HOME\}/conf/settings.xml：

``````````
<profiles><profile><id>sonar</id><properties><sonar.jdbc.url>jdbc:mysql://192.168.198.128:3306/sonar</sonar.jdbc.url><sonar.jdbc.driver>com.mysql.jdbc.Driver</sonar.jdbc.driver><sonar.jdbc.username>root</sonar.jdbc.username><sonar.jdbc.password>root</sonar.jdbc.password><sonar.host.url>http://localhost:9000</sonar.host.url><!-- Sonar服务器访问地址 --></properties></profile></profiles><activeProfiles><activeProfile>sonar</activeProfile></activeProfiles>
``````````

**3）执行mvn sonar:sonar命令进行代码分析。**

我们可以在Eclipse中，对一个标准maven工程执行sonar。说明：由于maven对sonar有很好的支持，会自动执行相应的脚本，所以无需在pom中添加sonar说明。

在执行maven进行sonar分析之前，必须确保sonar服务器已经处于运行状态。本例中sonar服务器运行在localhost:9000上。

首先，执行sonar:sonar命令，最后得到输出如下输出。如果输出”BUILD SUCCESS“说明已经构建成功。

![代码质量管理的开源平台Sonar][Sonar 1]

然后，我们可以在浏览器查看分析结果。

**查看分析结果**

对于使用sonar自带服务器来说，在浏览器访问：http://sonar\_ip:9000，打开sonar结果页面。可使用admin/admin账号登录进入。

**1）home页**

下面是home页，右边PROJECTS页面列出了所有的工程。点击红色框内的链接，可以查看详细情况。

![代码质量管理的开源平台Sonar][Sonar 2]

**2）工程总面板视图**

Dashboard包含了很多信息，比如程序统计信息、问题统计信息、技术债务、代码复杂度、单元测试覆盖度等。

![代码质量管理的开源平台Sonar][Sonar 3]

**3）Hotspots热点区**

在热点区，可以查看比较主要（hot）的信息。

![代码质量管理的开源平台Sonar][Sonar 4]

**4）问题视图**

点击左侧导航树的“问题”，打开问题视图页。通过点击问题数，如下红框所示，可以查看具体问题。

![代码质量管理的开源平台Sonar][Sonar 5]

点击问题数后，进入具体问题页。SonarQube允许管理员对问题进行重新确认，比如可以认为一个打开的问题是误判的。

![代码质量管理的开源平台Sonar][Sonar 6]

下面是认为一个问题是误判后的情况。

![代码质量管理的开源平台Sonar][Sonar 7]

在问题页面，可以通过“状态”搜索问题。下面是搜索“误判”问题的结果。

![代码质量管理的开源平台Sonar][Sonar 8]

**5）技术债务**

这里列出了修复问题所需要的时间，所谓技术债务。出来混总要还的，遗留的问题越多，技术债务越大。

![代码质量管理的开源平台Sonar][Sonar 9]

**6）问题明细**

这里列出问题明细，包括问题严重级别，对应的问题数量，问题的描述。

![代码质量管理的开源平台Sonar][Sonar 10]

**结合Jenkins**

可以将SONAR服务器放置在任意master或者slave节点上，在进行sonar分析时，必须在maven的conf/settings.xml中配置sonar服务器信息。然后就可以在jenkins中进行sonar分析了。

有两种方法使jenkins与sonar结合：一种就是上面介绍的通过maven（jenkins -maven - sonar），另外一种是直接在jenkins中调用sonar。


[Sonar]: http://p3.pstatp.com/large/pgc-image/1525741668463e49301c6ba
[Sonar 1]: http://p3.pstatp.com/large/pgc-image/15257416684403d83bf2dbf
[Sonar 2]: http://p1.pstatp.com/large/pgc-image/15257416685104c7c648c98
[Sonar 3]: http://p1.pstatp.com/large/pgc-image/1525741668494673391d969
[Sonar 4]: http://p1.pstatp.com/large/pgc-image/1525741668520482daaa620
[Sonar 5]: http://p9.pstatp.com/large/pgc-image/152574166843892c6b32427
[Sonar 6]: http://p3.pstatp.com/large/pgc-image/1525741668487e3df38d3bc
[Sonar 7]: http://p3.pstatp.com/large/pgc-image/15257416684691126ccbbbd
[Sonar 8]: http://p3.pstatp.com/large/pgc-image/1525741668493d1495c1339
[Sonar 9]: http://p3.pstatp.com/large/pgc-image/15257416699469e37992d88
[Sonar 10]: http://p3.pstatp.com/large/pgc-image/1525741668464d2e8ffb62a
 *  **原文作者：** 测试帮日记
 *  **原文链接：** https://www.toutiao.com/item/6553010622781456909/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。