---
title: SpringBoot-系统性能监测actuator
date: 2018-01-25 18:21:31
categories: "开发"
tags:
	- Java
	- Tomcat
	- 软件
	- 编程语言
	- JSON

---

**你想全方位的了解你运行中的系统吗？**

**你还对运行中的系统充满疑惑吗？**

**你想看你的系统垃圾什么时候gc回收吗？**

**走过路过，千万不要错过...**

**（1）首先导个jar**

``````````
<dependency><groupId>org.springframework.boot</groupId><artifactId>spring-boot-starter-actuator</artifactId></dependency>
``````````

（2）配置文件（.yml文件，请注意错行）

``````````
management:security:enabled: false
``````````

（3）原生端点分类

 *  应用配置类：获取应用程序中加载的应用配置、环境变量、自动化配置报告等与Spring Boot应用密切相关的配置类信息。
 *  度量指标类：获取应用程序运行过程中用于监控的度量指标，比如：内存信息、线程池信息、HTTP请求统计等。
 *  操作控制类：提供了对应用的关闭等操作类功能。

（4）具体接口

1）/health 返回的应用健康信息（示例代码网络粘贴，请包涵）

    | -------------------------------------------------------------------------------------------------------------------------- |
    | \{ "status": "UP", "diskSpace": \{ "status": "UP", "total": 491270434816, "free": 383870214144, "threshold": 10485760 \}\} |

 2）/autoconfig：该端点用来获取应用的自动化配置报告

    | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | \{ "positiveMatches": \{ // 条件匹配成功的 "EndpointWebMvcAutoConfiguration": \[ \{ "condition": "OnClassCondition", "message": "@ConditionalOnClass classes found: javax.servlet.Servlet,org.springframework.web.servlet.DispatcherServlet" \}, \{ "condition": "OnWebApplicationCondition", "message": "found web application StandardServletEnvironment" \} \], ... \}, "negativeMatches": \{ // 条件不匹配成功的 "HealthIndicatorAutoConfiguration.DataSourcesHealthIndicatorConfiguration": \[ \{ "condition": "OnClassCondition", "message": "required @ConditionalOnClass classes not found: org.springframework.jdbc.core.JdbcTemplate" \} \], ... \}\} |

3)/beans：该端点用来获取应用上下文中创建的所有Bean。


    | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | \[ \{ "context": "hello:dev:8881", "parent": null, "beans": \[ \{ "bean": "org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration$DispatcherServletConfiguration", "scope": "singleton", "type": "org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration$DispatcherServletConfiguration$$EnhancerBySpringCGLIB$$3440282b", "resource": "null", "dependencies": \[ "serverProperties", "spring.mvc.CONFIGURATION\_PROPERTIES", "multipartConfigElement" \] \}, \{ "bean": "dispatcherServlet", "scope": "singleton", "type": "org.springframework.web.servlet.DispatcherServlet", "resource": "class path resource \[org/springframework/boot/autoconfigure/web/DispatcherServletAutoConfiguration$DispatcherServletConfiguration.class\]", "dependencies": \[\] \} \] \}\] |



4)/configprops：该端点用来获取应用中配置的属性信息报告。

    | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | \{ "configurationPropertiesReportEndpoint": \{ "prefix": "endpoints.configprops", "properties": \{ "id": "configprops", "sensitive": true, "enabled": true \} \}, ...\} |

5)/env：该端点用来获取应用所有可用的环境属性报告。

    | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | \{ "profiles": \[ "dev" \], "server.ports": \{ "local.server.port": 8881 \}, "servletContextInitParams": \{ \}, "systemProperties": \{ "idea.version": "2016.1.3", "java.runtime.name": "Java(TM) SE Runtime Environment", "sun.boot.library.path": "C:\\Program Files\\Java\\jdk1.8.0\_91\\jre\\bin", "java.vm.version": "25.91-b15", "java.vm.vendor": "Oracle Corporation", ... \}, "systemEnvironment": \{ "configsetroot": "C:\\WINDOWS\\ConfigSetRoot", "RABBITMQ\_BASE": "E:\\tools\\rabbitmq", ... \}, "applicationConfig: \[classpath:/application-dev.properties\]": \{ "server.port": "8881" \}, "applicationConfig: \[classpath:/application.properties\]": \{ "server.port": "8885", "spring.profiles.active": "dev", "info.app.name": "spring-boot-hello", "info.app.version": "v1.0.0", "spring.application.name": "hello" \}\} |

6）/mappings：该端点用来返回所有Spring MVC的控制器映射关系报告。

``````````
{ "/webjars/**": { "bean": "resourceHandlerMapping" }, "/**": { "bean": "resourceHandlerMapping" }, "/**/favicon.ico": { "bean": "faviconHandlerMapping" }, "{[/hello]}": { "bean": "requestMappingHandlerMapping", "method": "public java.lang.String com.didispace.web.HelloController.index()" }, "{[/mappings || /mappings.json],methods=[GET],produces=[application/json]}": { "bean": "endpointHandlerMapping", "method": "public java.lang.Object org.springframework.boot.actuate.endpoint.mvc.EndpointMvcAdapter.invoke()" }, ...}
``````````


    |  |
    |  |

7）/info：该端点用来返回一些应用自定义的信息。（代码略）

8）/metrics：该端点用来返回当前应用的各类重要度量指标，比如：内存信息、线程信息、垃圾回收信息等。

``````````
{ "mem": 541305, "mem.free": 317864, "processors": 8, "instance.uptime": 33376471, "uptime": 33385352, "systemload.average": -1, "heap.committed": 476672, "heap.init": 262144, "heap.used": 158807, "heap": 3701248, "nonheap.committed": 65856, "nonheap.init": 2496, "nonheap.used": 64633, "nonheap": 0, "threads.peak": 22, "threads.daemon": 20, "threads.totalStarted": 26, "threads": 22, "classes": 7669, "classes.loaded": 7669, "classes.unloaded": 0, "gc.ps_scavenge.count": 7, "gc.ps_scavenge.time": 118, "gc.ps_marksweep.count": 2, "gc.ps_marksweep.time": 234, "httpsessions.max": -1, "httpsessions.active": 0, "gauge.response.beans": 55, "gauge.response.env": 10, "gauge.response.hello": 5, "gauge.response.metrics": 4, "gauge.response.configprops": 153, "gauge.response.star-star": 5, "counter.status.200.beans": 1, "counter.status.200.metrics": 3, "counter.status.200.configprops": 1, "counter.status.404.star-star": 2, "counter.status.200.hello": 11, "counter.status.200.env": 1}
``````````

从上面的示例中，我们看到有这些重要的度量值：

 *  系统信息：包括处理器数量`processors`、运行时间`uptime`和`instance.uptime`、系统平均负载`systemload.average`。
 *  `mem.*`：内存概要信息，包括分配给应用的总内存数量以及当前空闲的内存数量。这些信息来自`java.lang.Runtime`。
 *  `heap.*`：堆内存使用情况。这些信息来自`java.lang.management.MemoryMXBean`接口中`getHeapMemoryUsage`方法获取的`java.lang.management.MemoryUsage`。
 *  `nonheap.*`：非堆内存使用情况。这些信息来自`java.lang.management.MemoryMXBean`接口中`getNonHeapMemoryUsage`方法获取的`java.lang.management.MemoryUsage`。
 *  `threads.*`：线程使用情况，包括线程数、守护线程数（daemon）、线程峰值（peak）等，这些数据均来自`java.lang.management.ThreadMXBean`。
 *  `classes.*`：应用加载和卸载的类统计。这些数据均来自`java.lang.management.ClassLoadingMXBean`。
 *  `gc.*`：垃圾收集器的详细信息，包括垃圾回收次数`gc.ps_scavenge.count`、垃圾回收消耗时间`gc.ps_scavenge.time`、标记-清除算法的次数`gc.ps_marksweep.count`、标记-清除算法的消耗时间`gc.ps_marksweep.time`。这些数据均来自`java.lang.management.GarbageCollectorMXBean`。
 *  `httpsessions.*`：Tomcat容器的会话使用情况。包括最大会话数`httpsessions.max`和活跃会话数`httpsessions.active`。该度量指标信息仅在引入了嵌入式Tomcat作为应用容器的时候才会提供。
 *  `gauge.*`：HTTP请求的性能指标之一，它主要用来反映一个绝对数值。比如上面示例中的`gauge.response.hello: 5`，它表示上一次`hello`请求的延迟时间为5毫秒。
 *  `counter.*`：HTTP请求的性能指标之一，它主要作为计数器来使用，记录了增加量和减少量。如上示例中`counter.status.200.hello: 11`，它代表了`hello`请求返回`200`状态的次数为11。

9）/dump：该端点用来暴露程序运行中的线程信息。

10）/trace：该端点用来返回基本的HTTP跟踪信息。

![SpringBoot-系统性能监测actuator][SpringBoot-_actuator]

Cloud宣传


[SpringBoot-_actuator]: /pro/os/crawler/BUEM-MMEN-FAYV.jpg
 *  **原文作者：** 爆发的羔羊
 *  **原文链接：** https://www.toutiao.com/item/6514931482870415885/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。