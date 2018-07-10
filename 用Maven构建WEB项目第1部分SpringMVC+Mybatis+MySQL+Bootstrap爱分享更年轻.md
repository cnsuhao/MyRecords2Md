---
title: 用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap
date: 2018-05-04 11:23:02
categories: "开发"
tags:
	- Java
	- Tomcat
	- XML
	- MySQL
	- 编程语言

---

【原创超详细教程】利用Maven构建SpringMVC+Mybatis+MySQL+Bootstrap

本教程为入门教程，尽量使用详细的截图配合文字及代码，适合计算机专业在校学生、初入职场的初级java开发人员。

Maven + Spring MVC＋Mybatis + MySQL +AngularJS + Bootstrap

**1、准备工作：**

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap]

在Windows-preferences-maven中检查电脑中是否安装好maven

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 1]

在Windows-preferences-server中检查电脑中是否安装好Tomcat

**2、创建项目：**

开始创建项目

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 2]

File-new-maven project

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 3]

选择maven-archetype-webapp

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 4]

设置包名（Group Id）与项目名（Artifact Id）

点击finish后，maven会自动构建一个web项目，最初结构如下图：


![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 5]

index.jsp页面会报错：The superclass "javax.servlet.http.HttpServlet" was not found on the Java Build Path。这个错误是因为该JavaWeb工程类中没有添加Tomcat运行时相关类导致，在项目上右键-->Properties-->Java Build Path->Libraries--> Add Libray...-->Server Runtime -->Tomcat Server，完成后错误会自动消失。

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 6]

解决初始化index.jsp页面报错问题

然后根据MVC设计模式新建java文件夹为分层做准备


![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 7]

手动创建文件夹

至此，一个web项目的雏形就出现了，接下来需要利用maven配置pom文件来下载SpringMVC、mybatis等等相关jar包与配置文件。

**3、准备pom依赖：**

整合Spring、mybatis并与MySQL数据库连接


【pom.xml示例】

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4\_0\_0.xsd">

<modelVersion>4.0.0</modelVersion>

<groupId>com.bhyyc.official</groupId>

<artifactId>communitymarket</artifactId>

<packaging>war</packaging>

<version>0.0.1-SNAPSHOT</version>

<name>communitymarket Maven Webapp</name>

<url>http://maven.apache.org</url>

<properties>

<!-- spring版本号 -->

<spring.version>4.2.4.RELEASE</spring.version>

<!-- mybatis版本号 -->

<mybatis.version>3.4.1</mybatis.version>

</properties>

<dependencies>

<dependency>

<groupId>junit</groupId>

<artifactId>junit</artifactId>

<version>4.12</version>

<scope>test</scope>

</dependency>

<dependency>

<groupId>org.mybatis</groupId>

<artifactId>mybatis</artifactId>

<version>3.4.1</version>

</dependency>

<dependency>

<groupId>org.mybatis</groupId>

<artifactId>mybatis-spring</artifactId>

<version>1.3.0</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-core</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-beans</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<!-- Spring 在 3.2.13版本后，要单独引用 -->

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-oxm</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-tx</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-web</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-webmvc</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-jdbc</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-context-support</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<dependency>

<groupId>org.springframework</groupId>

<artifactId>spring-test</artifactId>

<version>$\{spring.version\}</version>

</dependency>

<!-- java ee (注解什么的都要用到它) -->

<dependency>

<groupId>javax</groupId>

<artifactId>javaee-api</artifactId>

<version>7.0</version>

</dependency>

<!-- dbcp用来在applicationContext.xml中配置数据库 -->

<dependency>

<groupId>commons-dbcp</groupId>

<artifactId>commons-dbcp</artifactId>

<version>1.4</version>

</dependency>

<!-- Mysql数据库链接 -->

<dependency>

<groupId>mysql</groupId>

<artifactId>mysql-connector-java</artifactId>

<version>6.0.2</version>

</dependency>

<!-- Spring 4.x 依赖的相关 json jar -->

<dependency>

<groupId>com.fasterxml.jackson.core</groupId>

<artifactId>jackson-databind</artifactId>

<version>2.5.4</version>

</dependency>

<dependency>

<groupId>com.fasterxml.jackson.core</groupId>

<artifactId>jackson-core</artifactId>

<version>2.5.4</version>

</dependency>

<dependency>

<groupId>com.fasterxml.jackson.core</groupId>

<artifactId>jackson-annotations</artifactId>

<version>2.5.4</version>

</dependency>

</dependencies>

<build>

<finalName>communitymarket</finalName>

<plugins>

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-compiler-plugin</artifactId>

<configuration>

<source>1.8</source>

<target>1.8</target>

</configuration>

</plugin>

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-war-plugin</artifactId>

<version>2.6</version>

<configuration>

<failOnMissingWebXml>false</failOnMissingWebXml>

</configuration>

</plugin>

</plugins>

</build>

</project>

**maven构建项目可能会出现的问题解决办法（没有出现相关问题可以忽略这部分，直接继续步骤4）**

 *  

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 8]

项目jre版本与eclipse默认最好一致

 *  解决jax-rs的问题

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 9]

java resource报错jax-rs问题

 *  解决Eclipse建Maven项目module无法转换为2.5
    

https://jingyan.baidu.com/article/fb48e8be3279766e622e1496.html

 *  找不到web.xml问题

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 10]

**4、JDBC、mybatis的配置：**

jdbc.properties的配置前需要本地电脑安装完成MySQL数据库

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 11]

数据库连接需要的配置与用户表示例

【jdbc.properties代码示例】

\#database

driver=com.mysql.cj.jdbc.Driver

jdbc.url=jdbc:mysql://127.0.0.1:3306/communitymarket?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8&useSSL=false

jdbc.username=root

jdbc.password=123456

jdbc.maxWait=5000

提醒: a、需要注意不要有拼写错误，b、url中的communitymarket为项目的MySQL数据库名称，c、复制时注意每行后面不要有空格d、com.mysql.cj.jdbc.Driver为新版本驱动类的路径，com.mysql.cj.jdbc.Driver是mysql-connector-java 6.2的新驱动名称，老的是com.mysql.jdbc.Driver。取决于你的mysql-connector-java-x.x.x.jar包的版本。

![用Maven构建WEB项目第1部分SpringMVC+Mybatis+MySQL+Bootstrap][Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 12]

反编译一下mysql-connector-java-x.x.x.jar看一下驱动类的路径

【spring-mybatis.xml示例】


<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:aop="http://www.springframework.org/schema/aop"

xmlns:tx="http://www.springframework.org/schema/tx"

xmlns:context="http://www.springframework.org/schema/context"

xsi:schemaLocation="

http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd

http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd

http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd

http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd">

<!-- 自动扫描的包 ,将带有注解的类 纳入spring容器管理 -->

<context:component-scan base-package="com.bhyyc.official"></context:component-scan>

<!-- 引入jdbc配置文件 -->

<bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">

<property name="locations">

<list>

<value>classpath\*:conf/jdbc.properties</value>

</list>

</property>

</bean>

<!-- dataSource 配置 -->

<bean id="sampleDataSource" class="org.apache.commons.dbcp.BasicDataSource">

<!-- 基本属性 url、user、password -->

<property name="driverClassName" value="$\{driver\}" />

<property name="url" value="$\{jdbc.url\}" />

<property name="username" value="$\{jdbc.username\}" />

<property name="password" value="$\{jdbc.password\}" />

<property name="maxWait" value="$\{jdbc.maxWait\}" />

</bean>

<!-- mybatis文件配置，扫描所有mapper文件 -->

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">

<property name="dataSource" ref="sampleDataSource"/>

<property name="mapperLocations" value="classpath:mappers/\*.xml"/>

</bean>

<!-- spring与mybatis整合配置，扫描所有dao -->

<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">

<property name="basePackage" value="com.bhyyc.official.dao"/>

<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>

</bean>

</beans>

如果无法识别驱动类，可以把相关mysql-connector-java-6.0.2.jar拷贝至Tomcat的lib目录例如 E:Tomcat7apache-tomcat-7.0.85lib

**5、MVC模式的dao层与service层代码示例：**

针对MVC模式的架构，Java代码分成实体层entity、数据访问层dao、业务服务层service（又分为接口和实现）和控制层controller。控制层是与前端交互打交道的，可以先不管。

这部分其余代码示例如下：

【User.java示例】


package com.bhyyc.official.entity;

public class User \{

private int id;

private String name;

private String password;

public int getId() \{

return id;

\}

public void setId(int id) \{

this.id = id;

\}

public String getName() \{

return name;

\}

public void setName(String name) \{

this.name = name;

\}

public String getPassword() \{

return password;

\}

public void setPassword(String password) \{

this.password = password;

\}

\}

【UserMapper.java示例】

package com.bhyyc.official.dao;

import org.apache.ibatis.annotations.Param;

import com.bhyyc.official.entity.User;

public interface UserMapper \{

User select(@Param("name")String name);

int userNameExits(@Param("name")String name);

int accountValid(User user);

int insert(User user);

\}

【userMapper.xml示例】

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"

"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper

namespace="com.bhyyc.official.dao.UserMapper">

<resultMap id="userMap" type="com.bhyyc.official.entity.User">

<id property="id" column="id" javaType="int"/>

<result property="name" column="name" javaType="String"/>

<result property="password" column="password" javaType="String"/>

</resultMap>

<select id="select" resultMap="userMap">

select \* from users

<where>

<if test="name!=null">

and name=\#\{name\}

</if>

</where>

</select>

<select id="userNameExits" resultType="int">

select count(0) from users where name=\#\{name\}

</select>

<select id="accountValid" parameterType="com.bhyyc.official.entity.User" resultType="int">

select count(0) from users where name=\#\{name\} and password=\#\{password\}

</select>

<insert id="insert" parameterType="com.bhyyc.official.entity.User" useGeneratedKeys="true" keyProperty="id">

insert into users (name, password) values (\#\{name\},\#\{password\})

</insert>

</mapper>

【UserService.java示例】

package com.bhyyc.official.service;

import com.bhyyc.official.entity.User;

public interface UserService \{

User select(String name);

int userNameExits(String name);

boolean accountValid(User user);

int insert(User user);

\}

【UserServiceImpl.java示例】

package com.bhyyc.official.service.impl;

import javax.annotation.Resource;

import org.springframework.stereotype.Service;

import com.bhyyc.official.dao.UserMapper;

import com.bhyyc.official.entity.User;

import com.bhyyc.official.service.UserService;

@Service

public class UserServiceImpl implements UserService\{

@Resource

private UserMapper userMapper;

public User select(String name) \{

return userMapper.select(name);

\}

public int userNameExits(String name) \{

return userMapper.userNameExits(name);

\}

public boolean accountValid(User user) \{

return userMapper.accountValid(user)>0;

\}

public int insert(User user) \{

return userMapper.insert(user);

\}

\}

【Junit简单测试一下users表里添加一条数据】

package communitymarket;

import javax.annotation.Resource;

import org.junit.Test;

import org.junit.runner.RunWith;

import org.springframework.test.context.ContextConfiguration;

import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import com.bhyyc.official.entity.User;

import com.bhyyc.official.service.UserService;

@RunWith(SpringJUnit4ClassRunner.class)

@ContextConfiguration(locations=\{"classpath:conf/spring-mybatis.xml"\})

public class UserTest \{

@Resource

UserService us;

@Test

public void testAdd()\{

User user = new User();

user.setName("test");

user.setPassword("testpd");

int res= us.insert(user);

System.out.println(res);

\}

@Test

public void testAccountValid()\{

User user = new User();

user.setName("test");

user.setPassword("testpd");

boolean res = us.accountValid(user);

System.out.println(res);

\}

\}

执行单元测试，测试结果打印出 testAdd方法打印出1，表示插入成功；testAccountValid方法打印出true，表示用户账号有效。

到此处，本教程的前半部分已经结束。

下一篇文章将继续web层的配置spring-mvc.xml与controllor层及bootstarp UI部分，感兴趣的同学们可以关注一下博主，大家一起学习进步。

第二部分见本头条号【爱分享更年轻】的下一篇文章【用Maven构建WEB项目第2部分SpringMVC+Mybatis+MySQL+Bootstrap】


[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap]: http://p1.pstatp.com/large/pgc-image/1525016636274fb8c826d2b
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 1]: http://p3.pstatp.com/large/pgc-image/15250169455210d97bc2d4a
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 2]: http://p3.pstatp.com/large/pgc-image/1525017177100845e8f6fa9
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 3]: http://p3.pstatp.com/large/pgc-image/15250175568435840cb6ab9
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 4]: http://p9.pstatp.com/large/pgc-image/1525018039217d1ffe0a1ee
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 5]: /pro/os/crawler/UMFY-IJRI-3ERJ.jpg
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 6]: /pro/os/crawler/ZEJN-JJBB-JRFA.jpg
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 7]: /pro/os/crawler/JQRV-QFQ3-YBME.jpg
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 8]: /pro/os/crawler/F32U-I3YM-NYIN.jpg
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 9]: /pro/os/crawler/VMI7-JJRV-2EVB.jpg
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 10]: /pro/os/crawler/3UQJ-IY7B-NYFQ.jpg
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 11]: http://p9.pstatp.com/large/pgc-image/1525399506993d5ab6a6d3c
[Maven_WEB_1_SpringMVC_Mybatis_MySQL_Bootstrap 12]: /pro/os/crawler/ZM3Y-II7B-IYVM.jpg
 *  **原文作者：** 爱分享更年轻
 *  **原文链接：** https://www.toutiao.com/item/6549894795232281095/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。