---
title: mybatis 多数据配置和切换
date: 2018-06-21 09:01:04
categories: "开发"
tags:
	- 技术

---

这里以在项目里配置dataSource、dataSource\_order、dataSource\_mycat这三个数据源为例。

1.首先要在mybatis的配置文件里配置这三个数据源的地址、用户名、密码等相关信息，这一步和单个数据源是一致的。

实例代码如下（我的mybatis配置文件为spring-mybatis.xml）：

<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"

init-method="init" destroy-method="close">

<property name="url" value="jdbc:mysql://127.0.0.1:3306/test\_office?useUnicode=true&characterEncoding=utf-8"/>

<property name="username" value="root"/>

<property name="password" value="test"/>

</bean>

<bean id="dataSource\_order" class="com.alibaba.druid.pool.DruidDataSource"

init-method="init" destroy-method="close">

<property name="url" value="jdbc:mysql://192.168.1.1:3306/test?useUnicode=true&characterEncoding=utf-8"/>

<property name="username" value="root"/>

<property name="password" value="test"/>

</bean>

<bean id="dataSource\_mycat" class="com.alibaba.druid.pool.DruidDataSource"

init-method="init" destroy-method="close">

<property name="url" value="jdbc:mysql://192.168.1.2:3306/TESTDB?useUnicode=true&characterEncoding=utf-8"/>

<property name="username" value="root"/>

<property name="password" value="root"/>

</bean>

2.在项目中加入以下几个类文件

具体代码如下：DataSourceAdvice

import com.beefly.office.constant.Constant;

public class DataSourceAdvice \{

public void sysDataSource() \{

DataSourceContextHolder.setDbType(Constant.SYS\_DATA\_SOURCE);

System.err.println(DataSourceContextHolder.getDbType());

\}

public void orderDataSource() \{

DataSourceContextHolder.setDbType(Constant.ORDER\_DATA\_SOURCE);

System.err.println(DataSourceContextHolder.getDbType());

\}

public void mycatDataSource() \{

DataSourceContextHolder.setDbType(Constant.MYCAT\_DATA\_SOURCE);

System.err.println(DataSourceContextHolder.getDbType());

\}

\}

DataSourceContextHolder代码如下：

public class DataSourceContextHolder \{

private static final ThreadLocal<String> contextHolder = new ThreadLocal<String>();

public static void setDbType(String dbType) \{

contextHolder.set(dbType);

\}

public static String getDbType() \{

return ((String) contextHolder.get());

\}

public static void clearDbType() \{

contextHolder.remove();

\}

\}

DynamicDataSource代码如下：

import java.util.logging.Logger;

import org.springframework.jdbc.datasource.lookup.AbstractRoutingDataSource;

public class DynamicDataSource extends AbstractRoutingDataSource \{

@Override

public Logger getParentLogger() \{

return null;

\}

@Override

protected Object determineCurrentLookupKey() \{

return DataSourceContextHolder.getDbType();

\}

\}

SqlSessionFactoryBeanHandler代码如下：

import java.io.IOException;

import org.apache.ibatis.executor.ErrorContext;

import org.apache.ibatis.session.SqlSessionFactory;

import org.mybatis.spring.SqlSessionFactoryBean;

import org.springframework.core.NestedIOException;

public class SqlSessionFactoryBeanHandler extends SqlSessionFactoryBean \{

@Override

protected SqlSessionFactory buildSqlSessionFactory() throws IOException \{

// TODO Auto-generated method stub

try \{

return super.buildSqlSessionFactory();

\} catch (NestedIOException e) \{

e.printStackTrace(); // XML 有错误时打印异常。

throw new NestedIOException("Failed to parse mapping resource: '" + "'", e);

\} finally \{

ErrorContext.instance().reset();

\}

\}

\}

3.在mybatis配置文件中加入动态数据源切换的bean

<bean id ="dynamicSource" class= "com.beefly.office.handler.DynamicDataSource">

<property name ="targetDataSources">

<map key-type ="java.lang.String">

<entry value-ref ="dataSource" key= "dataSource"></entry>

<entry value-ref ="dataSource\_order" key= "dataSource\_order"></entry>

<entry value-ref ="dataSource\_mycat" key= "dataSource\_mycat"></entry>

</map>

</property>

<property name ="defaultTargetDataSource" ref= "dataSource"></property> <!-- 默认使用dataSource的数据源 -->

</bean >

4.加入一个aop实现切换

<aop:config>

<aop:aspect ref="dataSourceRoute">

<aop:pointcut expression ="execution(\* com.beefly.office.dao.cxDao.\*.\*(..))" id= "pointcut" />

<aop:pointcut expression ="execution(\* com.beefly.office.dao.mycatDao.\*.\*(..))" id= "pointcut\_mycat" />

<aop:before method="orderDataSource" pointcut-ref="pointcut"/>

<aop:after method="sysDataSource" pointcut-ref="pointcut"/>

<aop:before method="mycatDataSource" pointcut-ref="pointcut\_mycat"/>

<aop:after method="sysDataSource" pointcut-ref="pointcut\_mycat"/>

</aop:aspect>

</aop:config>

5.全部mybatis配置为：

<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop" xmlns:cache="http://www.springframework.org/schema/cache"

xmlns:context="http://www.springframework.org/schema/context"

xsi:schemaLocation="http://www.springframework.org/schema/beans

http://www.springframework.org/schema/beans/spring-beans-4.0.xsd

http://www.springframework.org/schema/tx

http://www.springframework.org/schema/tx/spring-tx-4.0.xsd

http://www.springframework.org/schema/aop

http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">

<!-- <bean id="zkDataSource" class="com.beefly.common.jdbc.ZkDataSource" init-method="init" destroy-method="close">

<property name="connectProperties" value="\_jdbc\_config" />

</bean> -->

<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"

init-method="init" destroy-method="close">

<property name="url" value="jdbc:mysql://127.0.0.1:3306/test\_office?useUnicode=true&characterEncoding=utf-8"/>

<property name="username" value="root"/>

<property name="password" value="test"/>

</bean>

<bean id="dataSource\_order" class="com.alibaba.druid.pool.DruidDataSource"

init-method="init" destroy-method="close">

<property name="url" value="jdbc:mysql://192.168.1.1:3306/test?useUnicode=true&characterEncoding=utf-8"/>

<property name="username" value="root"/>

<property name="password" value="test"/>

</bean>

<bean id="dataSource\_mycat" class="com.alibaba.druid.pool.DruidDataSource"

init-method="init" destroy-method="close">

<property name="url" value="jdbc:mysql://192.168.1.2:3306/TESTDB?useUnicode=true&characterEncoding=utf-8"/>

<property name="username" value="root"/>

<property name="password" value="root"/>

</bean>

<bean id ="dynamicSource" class= "com.beefly.office.handler.DynamicDataSource">

<property name ="targetDataSources">

<map key-type ="java.lang.String">

<entry value-ref ="dataSource" key= "dataSource"></entry>

<entry value-ref ="dataSource\_order" key= "dataSource\_order"></entry>

<entry value-ref ="dataSource\_mycat" key= "dataSource\_mycat"></entry>

</map>

</property>

<property name ="defaultTargetDataSource" ref= "dataSource"></property> <!-- 默认使用dataSource的数据源 -->

</bean >

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">

<property name="configLocation" value="classpath:mybatis/mybatis-config.xml" />

<property name="mapperLocations" value="classpath:mapper/\*.xml" />

<property name="dataSource" ref="dynamicSource" />

<property name="typeAliasesPackage">

<value>

com.beefly.office.domain

</value>

</property>

</bean>

<bean class ="org.mybatis.spring.mapper.MapperScannerConfigurer">

<property name ="basePackage" value= "com.beefly.office.dao"></property >

</bean>

<bean id="dataSourceRoute" class="com.beefly.office.handler.DataSourceAdvice"></bean>

<aop:config>

<aop:aspect ref="dataSourceRoute">

<aop:pointcut expression ="execution(\* com.beefly.office.dao.cxDao.\*.\*(..))" id= "pointcut" />

<aop:pointcut expression ="execution(\* com.beefly.office.dao.mycatDao.\*.\*(..))" id= "pointcut\_mycat" />

<aop:before method="orderDataSource" pointcut-ref="pointcut"/>

<aop:after method="sysDataSource" pointcut-ref="pointcut"/>

<aop:before method="mycatDataSource" pointcut-ref="pointcut\_mycat"/>

<aop:after method="sysDataSource" pointcut-ref="pointcut\_mycat"/>

</aop:aspect>

</aop:config>

<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">

<property name="dataSource" ref="dynamicSource" />

</bean>

<!-- enable transaction demarcation with annotations -->

<tx:annotation-driven transaction-manager="transactionManager" />

<!-- enable transaction demarcation with annotations -->

<tx:annotation-driven />

</beans>

![mybatis 多数据配置和切换][mybatis]


[mybatis]: http://p9.pstatp.com/large/pgc-image/152954285817184d7e2eef8
 *  **原文作者：** 程序员小新人学习
 *  **原文链接：** https://www.toutiao.com/item/6569336581419696648/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。