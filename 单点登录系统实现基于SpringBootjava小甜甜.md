---
title: 单点登录系统实现基于SpringBoot
date: 2017-12-25 14:51:58
categories: "开发"
tags:
	- Java
	- NoSQL
	- 编程语言
	- Redis
	- Chrome

---

# 单点登录系统实现基于SpringBoot #

今天的干货有点湿，里面夹杂着我的泪水。可能也只有代码才能让我暂时的平静。通过本章内容你将学到单点登录系统和传统登录系统的区别，单点登录系统设计思路，Spring4 Java配置方式整合HttpClient，整合SolrJ ，HttpClient�����易教程。还在等什么？撸起袖子开始干吧！

效果图：8081端口是sso系统，其他两个8082和8083端口模拟两个系统。登录成功后检查Redis数据库中是否有值。

![单点登录系统实现基于SpringBoot][SpringBoot]

web前端免费资源群在下一张图片下方

技术：SpringBoot，SpringMVC，Spring，SpringData，Redis，HttpClient

说明：本章的用户登录注册的代码部分已经在SpringBoot基础入门中介绍过了，这里��会��复贴代码。

源码：见文章底部

SpringBoot基础入门：http://www.cnblogs.com/itdrag...

单点登录系统简介

![单点登录系统实现基于SpringBoot][SpringBoot 1]

575057881

在传统的系统，或者是只有一个服务器的系统中。Session在一个服务器中，各个模块都可以直接获取，只需登录一次就进入各个模块。若在服务器集群或者是分布式系统架构中，每个服务器之间的Session并不是共享的，这会��现每��模块都要登录的情况。这时候需要通过单点登录系统（Single Sign On）将用户信息存在Redis数据库中实现Session共享的效果。从而实现一次登录就可以访问所有相互信任的应用系统。

单点登录系统实现

Maven项目核心配置文件 pom.xml 需要在原来的基础上添加 httpclient和jedis jar包

``````````
<dependency> <!-- http client version is 4.5.3 --> <groupId>org.apache.httpcomponents</groupId> <artifactId>httpclient</artifactId> </dependency> <dependency> <!-- redis java client version is 2.9.0 --> <groupId>redis.clients</groupId> <artifactId>jedis</artifactId> </dependency>
``````````

Spring4 Java配置方式

这里，我们需要整合httpclient用于各服务之间的通讯(也可以用okhttp)。同时还需要整合redis用于存储用户信息(Session共享)。

在Spring3.x之前，一般在**应用的基本配置**用xml，比如数据源、资源文件等。**业务开发**用注解，比如Component，Service，Controller等。其实在Spring3.x的时候就已经提供了Java配置方式。现在的Spring4.x和SpringBoot都开始推荐使用Java配置方式配置bean。它可以使bean的结构更加的清晰。

整合 HttpClient

HttpClient 是 Apache Jakarta Common 下的子项目，用来提供高效的、最新的、功能丰富的支持 HTTP 协议的客户端编程工具包，并且它支持 HTTP 协议最新的版本和建议。

HttpClient4.5系列教程 : http://blog.csdn.net/column/d...

首先在src/main/resources 目录下创建 httpclient.properties 配置文件

``````````
#设置整个连接池默认最大连接数http.defaultMaxPerRoute=100#设置整个连接池最大连接数http.maxTotal=300#设置请求超时http.connectTimeout=1000#设置从连接池中获取到连接的最长时间http.connectionRequestTimeout=500#设置数据传输的最长时间http.socketTimeout=10000
``````````

然后在 src/main/java/com/itdragon/config 目录下创建 HttpclientSpringConfig.java 文件

这里用到了四个很重要的注解

@Configuration : 作用于类上，指明该类就相当于一个xml配置文件

@Bean : 作用于方法上，指明该方法相当于xml配置中的<bean>，注意方法名的命名规范

@PropertySource : 指定读取的配置文件，引入多个value=\{"xxx:xxx","xxx:xxx"\},ignoreResourceNotFound=true 文件不存在是忽略

@Value : 获取配置文件的值，该注解还有很多语法知识，这里暂时不扩展开

``````````
package com.itdragon.config;import java.util.concurrent.TimeUnit;import org.apache.http.client.config.RequestConfig;import org.apache.http.impl.client.CloseableHttpClient;import org.apache.http.impl.client.HttpClients;import org.apache.http.impl.client.IdleConnectionEvictor;import org.apache.http.impl.conn.PoolingHttpClientConnectionManager;import org.springframework.beans.factory.annotation.Autowired;import org.springframework.beans.factory.annotation.Value;import org.springframework.context.annotation.Bean;import org.springframework.context.annotation.Configuration;import org.springframework.context.annotation.PropertySource;import org.springframework.context.annotation.Scope;/*** @Configuration 作用于类上，相当于一个xml配置文件* @Bean 作用于方法上，相当于xml配置中的<bean>* @PropertySource 指定读取的配置文件* @Value 获取配置文件的值*/@Configuration@PropertySource(value = "classpath:httpclient.properties")public class HttpclientSpringConfig { @Value("${http.maxTotal}") private Integer httpMaxTotal; @Value("${http.defaultMaxPerRoute}") private Integer httpDefaultMaxPerRoute; @Value("${http.connectTimeout}") private Integer httpConnectTimeout; @Value("${http.connectionRequestTimeout}") private Integer httpConnectionRequestTimeout; @Value("${http.socketTimeout}") private Integer httpSocketTimeout; @Autowired private PoolingHttpClientConnectionManager manager; @Bean public PoolingHttpClientConnectionManager poolingHttpClientConnectionManager() { PoolingHttpClientConnectionManager poolingHttpClientConnectionManager = new PoolingHttpClientConnectionManager(); // 最大连接数 poolingHttpClientConnectionManager.setMaxTotal(httpMaxTotal); // 每个主机的最大并发数 poolingHttpClientConnectionManager.setDefaultMaxPerRoute(httpDefaultMaxPerRoute); return poolingHttpClientConnectionManager; } @Bean // 定期清理无效连接 public IdleConnectionEvictor idleConnectionEvictor() { return new IdleConnectionEvictor(manager, 1L, TimeUnit.HOURS); } @Bean // 定义HttpClient对象 注意该对象需要设置scope="prototype":多例对象 @Scope("prototype") public CloseableHttpClient closeableHttpClient() { return HttpClients.custom().setConnectionManager(this.manager).build(); } @Bean // 请求配置 public RequestConfig requestConfig() { return RequestConfig.custom().setConnectTimeout(httpConnectTimeout) // 创建连接的最长时间 .setConnectionRequestTimeout(httpConnectionRequestTimeout) // 从连接池中获取到连接的最长时间 .setSocketTimeout(httpSocketTimeout) // 数据传输的最长时间 .build(); }}
``````````

整合 Redis

SpringBoot官方其实提供了spring-boot-starter-redis pom 帮助我们快速开发，但我们也可以自定义配置，这样可以更方便地掌控。

Redis 系列教程 : http://www.cnblogs.com/itdrag...

首先在src/main/resources 目录下创建 redis.properties 配置文件

设置Redis主机的ip地址和端口号，和存入Redis数据库���的key以���存活时间。这里为了方便测试，存活时间设置的比较小。这里的配置是单例Redis。

``````````
redis.node.host=192.168.225.131redis.node.port=6379REDIS_USER_SESSION_KEY=REDIS_USER_SESSIONSSO_SESSION_EXPIRE=30
``````````

在src/main/java/com/itdragon/config 目录下创建 RedisSpringConfig.java 文件

``````````
package com.itdragon.config;import java.util.ArrayList;import java.util.List;import org.springframework.beans.factory.annotation.Value;import org.springframework.context.annotation.Bean;import org.springframework.context.annotation.Configuration;import org.springframework.context.annotation.PropertySource;import redis.clients.jedis.JedisPool;import redis.clients.jedis.JedisPoolConfig;import redis.clients.jedis.JedisShardInfo;import redis.clients.jedis.ShardedJedisPool;@Configuration@PropertySource(value = "classpath:redis.properties")public class RedisSpringConfig { @Value("${redis.maxTotal}") private Integer redisMaxTotal; @Value("${redis.node.host}") private String redisNodeHost; @Value("${redis.node.port}") private Integer redisNodePort; private JedisPoolConfig jedisPoolConfig() { JedisPoolConfig jedisPoolConfig = new JedisPoolConfig(); jedisPoolConfig.setMaxTotal(redisMaxTotal); return jedisPoolConfig; }  @Bean public JedisPool getJedisPool(){ // 省略第一个参数则是采用 Protocol.DEFAULT_DATABASE JedisPool jedisPool = new JedisPool(jedisPoolConfig(), redisNodeHost, redisNodePort); return jedisPool; } @Bean public ShardedJedisPool shardedJedisPool() { List<JedisShardInfo> jedisShardInfos = new ArrayList<JedisShardInfo>(); jedisShardInfos.add(new JedisShardInfo(redisNodeHost, redisNodePort)); return new ShardedJedisPool(jedisPoolConfig(), jedisShardInfos); }}
``````````

Service 层

在src/main/java/com/itdragon/service 目录下创建 UserService.java 文件，它负责三件事情

第一件事件：验证用户信息是否正确，并将登录成功的用户信息保存到Redis数据库中。

第二件事件：负责判断用户令牌是否过期，若没有则刷新令牌存活时间。

第三件事件：负责从Redis数据库中删除用户信息。

这里用到了一些工具类，不影响学习，可以从源码中直接获取。

``````````
package com.itdragon.service;import java.util.UUID;import javax.servlet.http.HttpServletRequest;import javax.servlet.http.HttpServletResponse;import javax.transaction.Transactional;import org.springframework.beans.factory.annotation.Autowired;import org.springframework.beans.factory.annotation.Value;import org.springframework.context.annotation.PropertySource;import org.springframework.stereotype.Service;import org.springframework.util.StringUtils;import com.itdragon.pojo.ItdragonResult;import com.itdragon.pojo.User;import com.itdragon.repository.JedisClient;import com.itdragon.repository.UserRepository;import com.itdragon.utils.CookieUtils;import com.itdragon.utils.ItdragonUtils;import com.itdragon.utils.JsonUtils;@Service@Transactional@PropertySource(value = "classpath:redis.properties")public class UserService {  @Autowired private UserRepository userRepository;  @Autowired private JedisClient jedisClient;  @Value("${REDIS_USER_SESSION_KEY}") private String REDIS_USER_SESSION_KEY;  @Value("${SSO_SESSION_EXPIRE}") private Integer SSO_SESSION_EXPIRE;  public ItdragonResult userLogin(String account, String password, HttpServletRequest request, HttpServletResponse response) { // 判断账号密码是否正确 User user = userRepository.findByAccount(account); if (!ItdragonUtils.decryptPassword(user, password)) { return ItdragonResult.build(400, "账号名或密码错误"); } // 生成token String token = UUID.randomUUID().toString(); // 清空密码和盐避免泄漏 String userPassword = user.getPassword(); String userSalt = user.getSalt(); user.setPassword(null); user.setSalt(null); // 把用户信息写入 redis jedisClient.set(REDIS_USER_SESSION_KEY + ":" + token, JsonUtils.objectToJson(user)); // user 已经是持久化对象，被保存在session缓存当中，若user又重新修改属性值，那么在提交事务时，此时 hibernate对象就会拿当前这个user对象和保存在session缓存中的user对象进行比较，如果两个对象相同，则不会发送update语句，否则会发出update语句。 user.setPassword(userPassword); user.setSalt(userSalt); // 设置 session 的过期时间 jedisClient.expire(REDIS_USER_SESSION_KEY + ":" + token, SSO_SESSION_EXPIRE); // 添加写 cookie 的逻辑，cookie 的有效期是关闭浏览器就失效。 CookieUtils.setCookie(request, response, "USER_TOKEN", token); // 返回token return ItdragonResult.ok(token); }  public void logout(String token) { jedisClient.del(REDIS_USER_SESSION_KEY + ":" + token); } public ItdragonResult queryUserByToken(String token) { // 根据token从redis中查询用户信息 String json = jedisClient.get(REDIS_USER_SESSION_KEY + ":" + token); // 判断是否为空 if (StringUtils.isEmpty(json)) { return ItdragonResult.build(400, "此session已经过期，请重新登录"); } // 更新过期时间 jedisClient.expire(REDIS_USER_SESSION_KEY + ":" + token, SSO_SESSION_EXPIRE); // 返回用户信息 return ItdragonResult.ok(JsonUtils.jsonToPojo(json, User.class)); }}
``````````

Controller 层

负责跳转登录页面跳转

``````````
package com.itdragon.controller;import org.springframework.stereotype.Controller;import org.springframework.ui.Model;import org.springframework.web.bind.annotation.RequestMapping;@Controllerpublic class PageController { @RequestMapping("/login") public String showLogin(String redirect, Model model) { model.addAttribute("redirect", redirect); return "login"; }}
``````````

负责用户的登录，退出，获取令牌的操作

``````````
package com.itdragon.controller;import javax.servlet.http.HttpServletRequest;import javax.servlet.http.HttpServletResponse;import org.springframework.beans.factory.annotation.Autowired;import org.springframework.stereotype.Controller;import org.springframework.web.bind.annotation.PathVariable;import org.springframework.web.bind.annotation.RequestMapping;import org.springframework.web.bind.annotation.RequestMethod;import org.springframework.web.bind.annotation.ResponseBody;import com.itdragon.pojo.ItdragonResult;import com.itdragon.service.UserService;@Controller@RequestMapping("/user")public class UserController {  @Autowired private UserService userService;  @RequestMapping(value="/login", method=RequestMethod.POST) @ResponseBody public ItdragonResult userLogin(String username, String password, HttpServletRequest request, HttpServletResponse response) { try { ItdragonResult result = userService.userLogin(username, password, request, response); return result; } catch (Exception e) { e.printStackTrace(); return ItdragonResult.build(500, ""); } }  @RequestMapping(value="/logout/{token}") public String logout(@PathVariable String token) { userService.logout(token); // 思路是从Redis中删除key，实际��况请和业务��辑结合 return "index"; }  @RequestMapping("/token/{token}") @ResponseBody public Object getUserByToken(@PathVariable String token) { ItdragonResult result = null; try { result = userService.queryUserByToken(token); } catch (Exception e) { e.printStackTrace(); result = ItdragonResult.build(500, ""); } return result; }}
``````````

视图层

一个简单的登录页面

``````````
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%><!doctype html><html lang="zh"> <head> <meta name="viewport" content="initial-scale=1.0, width=device-width, user-scalable=no" /> <meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> <meta http-equiv="X-UA-Compatible" content="IE=edge,Chrome=1" /> <meta http-equiv="X-UA-Compatible" content="IE=8" /> <title>欢迎登录</title> <link type="image/x-icon" href="images/favicon.ico" rel="shortcut icon"> <link rel="stylesheet" href="static/css/main.css" /> </head> <body> <div class="wrapper"> <div class="container"> <h1>Welcome</h1> <form method="post" onsubmit="return false;" class="form"> <input type="text" value="itdragon" name="username" placeholder="Account"/> <input type="password" value="123456789" name="password" placeholder="Password"/> <button type="button" id="login-button">Login</button> </form> </div> <ul class="bg-bubbles"> <li></li> <li></li> <li></li> <li></li> <li></li> <li></li> <li></li> <li></li> <li></li> <li></li> </ul> </div> <script type="text/javascript" src="static/js/jquery-1.10.1.min.js" ></script> <script type="text/javascript"> var redirectUrl = "${redirect}"; // 浏览器中返回的URL function doLogin() { $.post("/user/login", $(".form").serialize(),function(data){ if (data.status == 200) { if (redirectUrl == "") { location.href = "http://localhost:8082"; } else { location.href = redirectUrl; } } else { alert("登录失败，原因是：" + data.msg); } }); } $(function(){ $("#login-button").click(function(){ doLogin(); }); }); </script> </body></html>
``````````

HttpClient 基础语法

这里封装了get，post请求的方法

``````````
package com.itdragon.utils;import java.io.IOException;import java.net.URI;import java.util.ArrayList;import java.util.List;import java.util.Map;import org.apache.http.NameValuePair;import org.apache.http.client.entity.UrlEncodedFormEntity;import org.apache.http.client.methods.CloseableHttpResponse;import org.apache.http.client.methods.HttpGet;import org.apache.http.client.methods.HttpPost;import org.apache.http.client.utils.URIBuilder;import org.apache.http.entity.ContentType;import org.apache.http.entity.StringEntity;import org.apache.http.impl.client.CloseableHttpClient;import org.apache.http.impl.client.HttpClients;import org.apache.http.message.BasicNameValuePair;import org.apache.http.util.EntityUtils;public class HttpClientUtil {  public static String doGet(String url) { // 无参数get请求 return doGet(url, null); } public static String doGet(String url, Map<String, String> param) { // 带参��get请求 CloseableHttpClient httpClient = HttpClients.createDefault(); // 创建一个默认可关闭的Httpclient 对象 String resultMsg = ""; // 设置返回值 CloseableHttpResponse response = null; // 定义HttpResponse 对象 try { URIBuilder builder = new URIBuilder(url); // 创建URI,可以设置host，设置参数等 if (param != null) { for (String key : param.keySet()) { builder.addParameter(key, param.get(key)); } } URI uri = builder.build(); HttpGet httpGet = new HttpGet(uri); // 创建http GET请求 response = httpClient.execute(httpGet); // 执行请求 if (response.getStatusLine().getStatusCode() == 200) { // 判断返回状态为200则给返回值赋值 resultMsg = EntityUtils.toString(response.getEntity(), "UTF-8"); } } catch (Exception e) { e.printStackTrace(); } finally { // 不要忘记关闭 try { if (response != null) { response.close(); } httpClient.close(); } catch (IOException e) { e.printStackTrace(); } } return resultMsg; }  public static String doPost(String url) { // 无参数post请求 return doPost(url, null); } public static String doPost(String url, Map<String, String> param) {// 带参数post请求 CloseableHttpClient httpClient = HttpClients.createDefault(); // 创建一个默认可关闭的Httpclient 对象 CloseableHttpResponse response = null; String resultMsg = ""; try { HttpPost httpPost = new HttpPost(url); // 创建Http Post请求 if (param != null) { // 创建参数列表 List<NameValuePair> paramList = new ArrayList<NameValuePair>(); for (String key : param.keySet()) { paramList.add(new BasicNameValuePair(key, param.get(key))); } UrlEncodedFormEntity entity = new UrlEncodedFormEntity(paramList);// 模拟表单 httpPost.setEntity(entity); } response = httpClient.execute(httpPost); // 执行http请求 if (response.getStatusLine().getStatusCode() == 200) { resultMsg = EntityUtils.toString(response.getEntity(), "utf-8"); } } catch (Exception e) { e.printStackTrace(); } finally { try { if (response != null) { response.close(); } httpClient.close(); } catch (IOException e) { e.printStackTrace(); } } return resultMsg; } public static String doPostJson(String url, String json) { CloseableHttpClient httpClient = HttpClients.createDefault(); CloseableHttpResponse response = null; String resultString = ""; try { HttpPost httpPost = new HttpPost(url); StringEntity entity = new StringEntity(json, ContentType.APPLICATION_JSON); httpPost.setEntity(entity); response = httpClient.execute(httpPost); if (response.getStatusLine().getStatusCode() == 200) { resultString = EntityUtils.toString(response.getEntity(), "utf-8"); } } catch (Exception e) { e.printStackTrace(); } finally { try { if (response != null) { response.close(); } httpClient.close(); } catch (IOException e) { e.printStackTrace(); } } return resultString; }}
``````````

Spring 自定义拦截器

更多web前端免费学习视频qun:575057881

这里是另外一个项目 itdragon-service-test-sso 中的代码，

首先在src/main/resources/spring/springmvc.xml 中配置拦截器，设置那些请求需要拦截

``````````
<!-- 拦截器配置 --> <mvc:interceptors> <mvc:interceptor> <mvc:mapping path="/github/**"/> <bean class="com.itdragon.interceptors.UserLoginHandlerInterceptor"/> </mvc:interceptor> </mvc:interceptors>
``````````

然后在 src/main/java/com/itdragon/interceptors 目录下创建 UserLoginHandlerInterceptor.java 文件

``````````
package com.itdragon.interceptors;import javax.servlet.http.HttpServletRequest;import javax.servlet.http.HttpServletResponse;import org.springframework.beans.factory.annotation.Autowired;import org.springframework.util.StringUtils;import org.springframework.web.servlet.HandlerInterceptor;import org.springframework.web.servlet.ModelAndView;import com.itdragon.pojo.User;import com.itdragon.service.UserService;import com.itdragon.utils.CookieUtils;public class UserLoginHandlerInterceptor implements HandlerInterceptor { public static final String COOKIE_NAME = "USER_TOKEN"; @Autowired private UserService userService; @Override public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception { String token = CookieUtils.getCookieValue(request, COOKIE_NAME); User user = this.userService.getUserByToken(token); if (StringUtils.isEmpty(token) || null == user) { // 跳转到登录页面，把用户请求的url作为参数传递给登录页面。 response.sendRedirect("http://localhost:8081/login?redirect=" + request.getRequestURL()); // ��回false return false; } // 把用户信息放入Request request.setAttribute("user", user); // 返回值决定handler是否执行。true：执行，false：不执行。 return true; } @Override public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception { } @Override public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception { }}
``````````

可能存在的问题

SpringData 自动更新问题

SpringData 是基于Hibernate的。当User 已经是持久化对象，被保存在session缓存当中。若User又重新修改属性值，在提交事务时，此时hibernate对象就会拿当前这个User对象和保存在session缓存中的User对象进行比较，如果两个对象相同，则不会发送update语句，否则，会发出update语句。

笔者采用比较傻的方法，就是在提交事务之��把数据还原。各位��果有更好的办法请告知，谢谢！

检查用户信息是否保存

登录成功后，进入Redis客户端查看用户信息是否保存成功。同时为了方便测试，也可以删除这个key。

``````````
[root@localhost bin]# ./redis-cli -h 192.168.225.131 -p 6379192.168.225.131:6379>192.168.225.131:6379> keys *1) "REDIS_USER_SESSION:1d869ac0-3d22-4e22-bca0-37c8dfade9ad"192.168.225.131:6379> get REDIS_USER_SESSION:1d869ac0-3d22-4e22-bca0-37c8dfade9ad"{"id":3,"account":"itdragon","userName":"ITDragonGit","plainPassword":null,"password":null,"salt":null,"iphone":"12349857999","email":"itdragon@git.com","platform":"github","createdDate":"2017-12-22 21:11:19","updatedDate":"2017-12-22 21:11:19"}"
``````````


[SpringBoot]: /pro/os/crawler/NUVZ-YFMF-IZJV.jpg
[SpringBoot 1]: /pro/os/crawler/JZVB-JQE2-AUAE.jpg
 *  **原文作者：** java小甜甜
 *  **原文链接：** https://www.toutiao.com/item/6503130243136487949/
 *  **版权声明：** ��博客所有文章除特别��明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。