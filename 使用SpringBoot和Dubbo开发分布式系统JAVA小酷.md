---
title: 使用SpringBoot和Dubbo开发分布式系统
date: 2018-04-01 15:12:18
categories: "开发"
tags:
	- Tomcat
	- 编译器
	- 技术
	- XML

---

首先我们先建立一个文件夹 demo 作为项目的存放目录

其次我们建立三个maven的jar项目

分别为 demo-lib, demo-db, demo-web

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo]

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo 1]

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo 2]

lib 用于 提供web和db互通的数据接口和接口

web 用于 提供网站访问

db 用于提供数据支持

我们为了便于管理 使用 maven 的 parent 配置, 在demo 文件夹中 建立一个 pom.xml 文件 并写入以下内容:

``````````
<?xml version="1.0" encoding="UTF-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion> <groupId>com.demo</groupId> <artifactId>demo-parent</artifactId> <version>1.0</version> <packaging>pom</packaging> <name>${project.artifactId}</name> <description>The parent project of demo</description> <parent> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-parent</artifactId> <version>1.5.3.RELEASE</version> <relativePath/> </parent> <modules> <module>demo-lib</module> <module>demo-db</module> <module>demo-web</module> </modules> <properties> <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> <maven.compiler.source>1.8</maven.compiler.source> <maven.compiler.target>1.8</maven.compiler.target> </properties> <dependencies> <dependency> <groupId>com.alibaba</groupId> <artifactId>dubbo</artifactId> <version>2.5.4-SNAPSHOT</version> <exclusions>  <exclusion>  <artifactId>spring</artifactId>  <groupId>org.springframework</groupId>  </exclusion>  </exclusions>  </dependency> <dependency> <groupId>com.google.guava</groupId> <artifactId>guava</artifactId> <version>21.0</version> </dependency> <dependency> <groupId>com.google.code.gson</groupId> <artifactId>gson</artifactId> </dependency> <dependency> <groupId>org.apache.zookeeper</groupId> <artifactId>zookeeper</artifactId> <version>3.3.3</version> </dependency> <dependency> <groupId>com.github.sgroschupf</groupId> <artifactId>zkclient</artifactId> <version>0.1</version> </dependency> <dependency> <groupId>com.thoughtworks.xstream</groupId> <artifactId>xstream</artifactId> <version>1.4.9</version> </dependency> <dependency> <groupId>com.alibaba</groupId> <artifactId>fastjson</artifactId> <version>1.2.24</version> </dependency> </dependencies> <build> <plugins> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-compiler-plugin</artifactId> <version>3.1</version> <configuration> <compilerArgument>-parameters</compilerArgument> <source>${maven.compiler.source}</source> <target>${maven.compiler.target}</target> </configuration> </plugin> </plugins> </build> <repositories> <repository> <id>spring-milestones</id> <name>Spring Milestones</name> <url>https://repo.spring.io/libs-milestone</url> <snapshots> <enabled>false</enabled> </snapshots> </repository> </repositories></project>
``````````

其次 demo-lib , demo-db , demo-web 分别配置 pom.xml

``````````
<!-- demo-lib 的 pom.xml 配置 --><?xml version="1.0" encoding="UTF-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion> <parent> <groupId>com.demo</groupId> <artifactId>demo-parent</artifactId> <version>1.0</version> </parent> <artifactId>demo-lib</artifactId> <packaging>jar</packaging></project><!-- demo-db 的 pom.xml 配置 --><?xml version="1.0" encoding="UTF-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion> <parent> <groupId>com.demo</groupId> <artifactId>demo-parent</artifactId> <version>1.0</version> </parent> <artifactId>demo-db</artifactId> <packaging>jar</packaging> <dependencies> <dependency> <groupId>com.demo</groupId> <artifactId>demo-lib</artifactId> <version>${project.parent.version}</version> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter</artifactId> </dependency> </dependencies> <build> <plugins> <plugin> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-maven-plugin</artifactId> </plugin> </plugins> </build></project><!-- demo-web 的 pom.xml 配置 --><?xml version="1.0" encoding="UTF-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> <modelVersion>4.0.0</modelVersion> <parent> <groupId>com.demo</groupId> <artifactId>demo-parent</artifactId> <version>1.0</version> </parent> <artifactId>demo-web</artifactId> <packaging>jar</packaging> <dependencies> <dependency> <groupId>com.demo</groupId> <artifactId>demo-lib</artifactId> <version>${project.parent.version}</version> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-web</artifactId> <exclusions> <exclusion> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-tomcat</artifactId> </exclusion> </exclusions> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-undertow</artifactId> </dependency> </dependencies> <build> <finalName>demo-web</finalName>  <defaultGoal>compile</defaultGoal> <plugins> <plugin> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-maven-plugin</artifactId> </plugin> </plugins> </build></project>
``````````

配置好了pom.xml文件后 项目结构就出来了

现在,我们在 Lib 中建立一个 Data 名为 Test (注意:结构需要可序列化 Serializable)

再建立一个 接口 为 TestService

在db模块中实现TestServiceImpl

在web中使用TestService

db模块中实现后需要注册到zookeeper中 需要设置 提供方的配置文件:

``````````
<?xml version="1.0" encoding="UTF-8"?><beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd"> <!-- 提供方应用信息，用于计算依赖关系 --> <dubbo:application name="provider-of-demo" /> <!-- 使用multicast广播注册中心暴露服务地址 --> <dubbo:registry address="zookeeper://192.168.80.251:2181?backup=192.168.80.252:2181,192.168.80.253:2181,192.168.80.254:2181" /> <!-- 用dubbo协议在20880端口暴露服务,同一个机子上启动多个需要配置不同的 port --> <dubbo:protocol name="dubbo" port="20881" /> <!-- 和本地bean一样实现服务 --> <bean id="testService" class="com.demo.db.service.TestServiceImpl" /> <!-- 声明需要暴露的服务接口 --> <dubbo:service interface="com.demo.lib.service.TestService" ref="testService" /></beans>
``````````

web模块使用需要 设置 使用方的 配置文件:

``````````
<?xml version="1.0" encoding="UTF-8"?><beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd"> <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 --> <dubbo:application name="web-of-demo" /> <!-- 使用multicast广播注册中心暴露发现服务地址 --> <dubbo:registry address="zookeeper://192.168.80.253:2181?backup=192.168.80.252:2181,192.168.80.251:2181,192.168.80.254:2181" /> <!-- 生成远程服务代理，可以和本地bean一样使用demoService --> <dubbo:reference id="TestService" interface="com.demo.lib.service.TestService" /></beans>
``````````

完成后使用SpringBoot的启动方式启动:

db模块的启动方式:

``````````
@SpringBootApplication@ImportResource("classpath:provider.xml")public class Launcher { public static void main(String[] args) { SpringApplication.run(Launcher.class, args); Thread t = new Thread(() -> { while (true) { try { Thread.sleep(Long.MAX_VALUE); } catch (Exception e) { } } }); t.start(); }}
``````````

web模块的启动方式:

``````````
@SpringBootApplication@ImportResource("classpath:consumer.xml")public class Launcher { public static void main(String[] args) { SpringApplication.run(Launcher.class, args); }}
``````````

启动完成后的测试效果:

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo 3]

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo 4]

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo 5]

![使用SpringBoot和Dubbo开发分布式系统][SpringBoot_Dubbo 6]


[SpringBoot_Dubbo]: /pro/os/crawler/2QN7-NAQA-FBQE.jpg
[SpringBoot_Dubbo 1]: /pro/os/crawler/IAIB-Y2MI-JJIZ.jpg
[SpringBoot_Dubbo 2]: /pro/os/crawler/BJF3-AUVY-RJ3Y.jpg
[SpringBoot_Dubbo 3]: /pro/os/crawler/MIUQ-NUEF-BN63.jpg
[SpringBoot_Dubbo 4]: http://p3.pstatp.com/large/pgc-image/1522566730524d3e5ee7b77
[SpringBoot_Dubbo 5]: /pro/os/crawler/FQYY-NVQV-IVAN.jpg
[SpringBoot_Dubbo 6]: http://p1.pstatp.com/large/pgc-image/1522566730528684bebc3d1
 *  **原文作者：** JAVA小酷
 *  **原文链接：** https://www.toutiao.com/item/6539374347197350414/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。