---
title: MyBatis集成自动化代码生成工具Generator进阶开发
date: 2017-11-08 07:34:10
categories: "开发"
tags:
	- 程序员
	- Java
	- XML
	- 编程语言
	- SQL

---

![MyBatis集成自动化代码生成工具Generator进阶开发][MyBatis_Generator]

进阶开发

# MyBatis #

> 一个支持自定义SQL、存储过程的一流的持久层框架，它消除了几乎所有的JDBC连接、参数以及检索结果设置，通过使用简单的XML、操作接口和POJO来完成对数据库的操作。

虽然MyBatis帮我们解决了很多JDBC相关的代码，但它是半自动化的，具体的SQL语句、映射关系还要手动配置，具体的POJO类还要自己去写。程序员是越来越懒的，当然不能把大部分时间花费到写这些无聊的代码上，因此MyBatisGenerator这样的工具就别开发出来了。

# MyBatisGenerator #

MyBatisGenerator是一个MyBatis的代码生成器，MBG试图帮助你完成几乎所有简单的数据库增删改查操作，但数据库的连接查询（一些项目并不建议使用join等连接查询）、存储过程还是需要手工去编写SQL和对象信息。虽然MyBatisGenerator并不能帮我们完成所有数据库操作相关的封装，但确实能够帮助我们免去写很多的代码。下面就开始我们的代码生产进阶之旅吧！！！

# 项目准备    #

依然使用Maven来构建项目。

1、引入Maven依赖

![MyBatis集成自动化代码生成工具Generator进阶开发][MyBatis_Generator 1]

Maven依赖

> 【说明】
> 
> pagehelper：使用查询分页时需要
> 
> spring-jdbc：使用事务（注解）时需要

2、配置Generator

![MyBatis集成自动化代码生成工具Generator进阶开发][MyBatis_Generator 2]

Generator配置文件

# 配置文件详解 #

【插件说明】  


> CaseInsensitiveLikePlugin：为生成大小写敏感的LIKE方法
> 
> SerializablePlugin：生成的Java模型类添加序列化接口，并生成serialVersionUID字段
> 
> MyBatis同时还提供了很多内置的插件，比如：CachePlugin、EqualsHashCodePlugin、MapperConfigPlugin、ToStringPlugin、RowBoundsPlugin等，当然还可以自定义一些符合自己业务的接口。

【注释配置 - commentGenerator】  


> suppressDate：（true） 阻止生成的注释包含时间戳
> 
> suppressAllComments：（true） 阻止生成的注释

【连接数据库】  


> 通过jdbcConnection来配置连接数据库，需要指定驱动、数据库地址、用户名和密码。

【生产的代码文件配置】

> 通过javaModelGenerator来配置实体类Model，通过sqlMapGenerator来配置SQLMapper（XML文件），通过javaClientGenerator来配置生成的接口文件。

【数据库表】

> 通过table来配置要生成的数据表，tableName指定数据库的表名，domainObjectName指定生产的实体类名称。

【主键获取】

> 通过配置 <generatedKey column="id" sqlStatement="MySql" identity="true"/> 在插入数据时返回主键信息，使用这段代码最终会在Mapper中的insert相关方法里面生成如下代码：
> 
> <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
> 
> SELECT LAST\_INSERT\_ID()
> 
> </selectKey>

# 开发使用 #

【运行】  


Generator自带一个运行JAVA脚本类org.mybatis.generator.api.ShellRunner，可通过运行ShellRunner自动生成代码文件。默认情况下时不能够自动覆盖生产的Mapper文件，每次生产时会将代码追加到之前的文件尾部。可以通过参数来实现覆盖，参数形式大致如下：

> args = new String\[\] \{ "-configfile", "src\\main\\resources\\mybatis-generator-config.xml", "-overwrite", "-verbose" \};

![MyBatis集成自动化代码生成工具Generator进阶开发][MyBatis_Generator 3]

运行结果

【代码使用】  


1、基础使用，可以使用生成得Mapper接口进行一些增删改查操作，如下图所示：

![MyBatis集成自动化代码生成工具Generator进阶开发][MyBatis_Generator 4]

基本增删改查

2、可以通过一下代码方法获取插入数据时的主键信息。  


> int userId = userMapper.insert(user);

3、可以使用PageHelper类进行分页查询（使用PageHelper时，紧跟在这个方法后的第一个MyBatis 查询方法会被进行分页，具体使用可以参考 https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/en/HowToUse.md）  


> PageHelper.startPage(page, offsize);


[MyBatis_Generator]: /pro/os/crawler/MEQ6-ZAVB-YVUV.jpg
[MyBatis_Generator 1]: /pro/os/crawler/AYIU-AJRJ-6ZUF.jpg
[MyBatis_Generator 2]: /pro/os/crawler/RNZQ-NR3Q-ZZV2.jpg
[MyBatis_Generator 3]: /pro/os/crawler/ZAZM-M2EM-A3AQ.jpg
[MyBatis_Generator 4]: /pro/os/crawler/FZAV-RYYE-BJYQ.jpg
 *  **原文作者：** 编程技术栈
 *  **原文链接：** https://www.toutiao.com/item/6485570496380273165/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。