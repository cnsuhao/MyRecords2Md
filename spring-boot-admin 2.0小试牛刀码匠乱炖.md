---
title: spring-boot-admin 2.0小试牛刀
date: 2018-05-29 21:15:48
categories: "开发"
tags:
	- 技术

---

**序**

本文主要展示下spring-boot-admin 2.0版本的新特性

**server实例**

**maven**

``````````
<dependency> <groupId>de.codecentric</groupId> <artifactId>spring-boot-admin-starter-server</artifactId> <version>2.0.0</version> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-webflux</artifactId> </dependency> <dependency> <groupId>org.jolokia</groupId> <artifactId>jolokia-core</artifactId> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-actuator</artifactId> </dependency>
``````````

**配置**

``````````
spring: application: name: spring-boot-admin-servereureka: instance: preferIpAddress: true leaseRenewalIntervalInSeconds: 10 client: registryFetchIntervalSeconds: 5 serviceUrl: defaultZone: ${EUREKA_SERVICE_URL:http://localhost:8761}/eureka/management: endpoints: web: exposure: include: "*" endpoint: health: show-details: ALWAYS
``````````

**config**

``````````
@Configuration@EnableAutoConfiguration@EnableAdminServerpublic class AdminServerApplication { public static void main(String[] args) { SpringApplication.run(AdminServerApplication.class, args); }}
``````````

**client实例**

**maven**

``````````
<dependency> <groupId>de.codecentric</groupId> <artifactId>spring-boot-admin-starter-client</artifactId> <version>2.0.0</version> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-actuator</artifactId> </dependency> <dependency> <groupId>org.jolokia</groupId> <artifactId>jolokia-core</artifactId> </dependency>
``````````

**配置**

``````````
spring: boot: admin: client: url: http://localhost:8080
``````````

> 这里配置admin server的地址

**运行实例**

**wallboard**

![spring-boot-admin 2.0小试牛刀][spring-boot-admin 2.0]

wallboard.png

**wallboard 实例详�����**

![spring-boot-admin 2.0小试牛刀][spring-boot-admin 2.0 1]

details.png

**applications**

![spring-boot-admin 2.0小试牛刀][spring-boot-admin 2.0 2]

applications.png

**journal**

![spring-boot-admin 2.0小试牛刀][spring-boot-admin 2.0 3]

journal.png

**小结**

新版前端改用vue.js进行了重构，后端的话，使用event sourcing的原则进行了重构，支持spring5，移除了spring-cloud-starter依赖，另外使用WebClient替代了zuul等等，具体详��spring-boot-admin-changes-with-2-x。

> 对于client端来说，目前还不能像1.x版本那样依靠Spring Cloud Discovery进行自动注册，目前需要使用spring-boot-admin-starter-client。

**doc**

 *  spring-boot-admin-changes-with-2-x
 *  spring-boot-admin/releases/tag/2.0.0


[spring-boot-admin 2.0]: /pro/os/crawler/R3QN-UUMJ-EEAJ.jpg
[spring-boot-admin 2.0 1]: /pro/os/crawler/I6BA-JR3M-BYRY.jpg
[spring-boot-admin 2.0 2]: /pro/os/crawler/NAII-N3YU-RE6J.jpg
[spring-boot-admin 2.0 3]: /pro/os/crawler/FAJQ-NFZJ-QRI3.jpg
 *  **原文作者：** 码匠乱炖
 *  **原文链接：** https://www.toutiao.com/item/6560990959092367880/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。