---
title: Spring整合Mybatis开发公司员工管理系统
date: 2018-06-24 21:18:27
categories: "开发"
tags:
	- Java
	- XML
	- 软件
	- 编程语言
	- WebApp

---

**1、建立工程**

1、需要建立父工程erpsm。

2、建立子modul：erpsm\_service（作为我们服务的核心）和erpsm\_web（作为我们页面的核心）

**2、引入pom依赖**

为了让工程之间有依赖，所以在pom文件中写入：

erpsm\_web：为了让web可以调用service中的方法

![Spring整合Mybatis开发公司员工管理系统][Spring_Mybatis]

erpsm\_service引入：我们业务代码的核心内容

``````````
<dependencies> <!--链接数据库--> <dependency> <groupId>mysql</groupId> <artifactId>mysql-connector-java</artifactId> <version>8.0.11</version> </dependency> <!--mybatis--> <dependency> <groupId>org.mybatis</groupId> <artifactId>mybatis</artifactId> <version>3.4.6</version> </dependency> <!--spring核心--> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-core</artifactId> <version>${spring.version}</version> </dependency> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-beans</artifactId> <version>${spring.version}</version> </dependency> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-context</artifactId> <version>${spring.version}</version> </dependency> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-aop</artifactId> <version>${spring.version}</version> </dependency> <!--可能会用到aspectj--> <dependency> <groupId>org.aspectj</groupId> <artifactId>aspectjweaver</artifactId> <version>1.9.1</version> </dependency> <!--链接数据库--> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-jdbc</artifactId> <version>${spring.version}</version> </dependency> <dependency> <groupId>org.springframework</groupId> <artifactId>spring-tx</artifactId> <version>${spring.version}</version> </dependency> <!--spring-mybatis整合--> <dependency> <groupId>org.mybatis</groupId> <artifactId>mybatis-spring</artifactId> <version>1.3.2</version> </dependency></dependencies>
``````````

**3、配置Mybatis的xml文件**

``````````
<?xml version="1.0" encoding="UTF-8" ?><beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd"> <!--整合mybatis--> <!--数据库链接信息--> <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource"> <property name="driverClassName" value="com.mysql.jdbc.Driver"/> <!-- & 在XML中需要转义--> <property name="url" value="jdbc:mysql//localhost:3306/erpsm?useUnicode=true&characterEncoding=utf-8"/> <property name="username" value="root"/> <property name="password" value="root"/> </bean> <!--sqlSession--> <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"> <property name="dataSource" ref="dataSource"/> <property name="typeAliasesPackage" value="com.imooc.io"/> </bean> <!--mapper扫描--> <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"> <property name="basePackage" value="com.imooc.dao"/> <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/> </bean> <!--事务申明--> <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> <property name="dataSource" ref="dataSource"/> </bean> <!--如果是get/get/search等关键词开头的方法，则只只读，其他才使用事务--> <tx:advice id="txAdvice" transaction-manager="transactionManager"> <tx:attributes> <tx:method name="get*" read-only="true"/> <tx:method name="find*" read-only="true"/> <tx:method name="search*" read-only="true"/> <tx:method name="*" propagation="REQUIRED"/> </tx:attributes> </tx:advice> <!--切入信息--> <aop:config> <!--为com.imooc.service这个包下的所有方法设置临时id txPointCut--> <aop:pointcut id="txPointCut" expression="execution(* com.imooc.service.*.*(..))"/> <!--当运行临时id为txPointCut的方法时，采用通知txPointCut进行匹配--> <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/> </aop:config> <!--打开自动扫描--> <context:component-scan base-package="com.imooc"/> <!--打开aspectJ--> <aop:aspectj-autoproxy/></beans>
``````````

**4、解决编码问题**

利用过滤器来设置编码的格式问题。

Java代码如下

``````````
package com.imooc.global;import org.apache.commons.lang3.StringUtils;import javax.servlet.*;import java.io.IOException;/** * @Auther: 封玉书 FYS * @Date: 2018.6.23 19:17 * @Description: 实现UTF-8过滤器，解决编码问题 */public class EncodeFilter implements Filter{ // 获取xml中，用户配置的ENCODE字段作为编码格式 private String ENCODE = "ENCODE"; // 接收编码格式 private String ENCODETYPE; // 默认的编码格式 private String UTF_8 = "UTF-8"; @Override public void init(FilterConfig filterConfig) throws ServletException { // 获取配置的编码格式 String encode = filterConfig.getInitParameter(ENCODE); // 当xml中配置的编码格式才进行设置，否则按默认的UTF-8 ENCODETYPE = StringUtils.isNotEmpty(encode) ? encode : UTF_8; }  @Override public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException { // 设置请求中的编码 servletRequest.setCharacterEncoding(ENCODETYPE); // �����置相应中的编码 servletResponse.setCharacterEncoding(ENCODETYPE); // 这里只做编码，所以直接放行业务的处理 filterChain.doFilter(servletRequest, servletResponse); } @Override public void destroy() { // 销毁编码 ENCODETYPE = null; }}
``````````

这里还需要有XML的配置。

``````````
<?xml version="1.0" encoding="UTF-8"?><web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1"> <!--申明刚才写的Java过滤器--> <filter> <filter-name>EncodingFilter</filter-name> <filter-class>com.imooc.global.EncodeFilter</filter-class> <init-param> <!--这里的参数名需要和java中定义的名字一致--> <param-name>ENCODE</param-name> <!--指定默认的编码格式--> <param-value>UTF-8</param-value> </init-param> </filter> <!--刚才的过滤器需要过滤哪些请求路径--> <filter-mapping> <filter-name>EncodingFilter</filter-name> <!--这里是所有--> <url-pattern>/*</url-pattern> </filter-mapping></web-app>
``````````


[Spring_Mybatis]: /pro/os/crawler/F7JM-ZRUF-VMMY.jpg
 *  **原文作者：** Java高级架构技术
 *  **原文链接：** https://www.toutiao.com/item/6570632695980753415/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。