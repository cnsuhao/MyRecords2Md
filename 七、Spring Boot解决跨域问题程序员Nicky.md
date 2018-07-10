---
title: 七、Spring Boot解决跨域问题
date: 2018-06-23 20:17:17
categories: "开发"
tags:
	- Origin
	- 技术
	- XML

---

一：问题描述

跨域问题指的是浏览器的同源策略导致页面访问服务器报错的一系列问题。而同源策略指的是以下方式：

**DOM同源策略**：禁止对不同源页面DOM进行操作。这里主要场景是iframe跨域的情况，不同域名的iframe是限制互相访问的。

**XmlHttpRequest同源策略**：禁止使用XHR对象向不同源的服务器地址发起HTTP请求。

二：解决方案

1：在服务器响应的时候加上response.setHeader("Access-Control-Allow-Origin", "\*");

2：客户端与服务器端同时加一个callBcak函数，jsonp

3：利用spring4.2的新特性

3.1:xml配置

<!-- 跨域 -->

<mvc:cors>

<mvc:mapping path="/\*\*" />

</mvc:cors>

3.2：注解

@CrossOrigin

4.springBoot结合拦截器配置

4.1、定义一个Interceptor拦截器

借用拦截器的preHandle方法，预先处理客户端的各种请求。

``````````
public class CrossInterceptor implements HandlerInterceptor {  @Override public boolean preHandle(HttpServletRequest httpServletRequest,  HttpServletResponse httpServletResponse, Object o) throws Exception {  // 跨域资源共享（ cors ） String origin = httpServletRequest.getHeader("Origin"); httpServletResponse.setHeader("Access-Control-Allow-Origin", origin); //允许的方法 httpServletResponse.setHeader("Access-Control-Allow-Methods", "*"); //允许的头部参数 httpServletResponse.setHeader("Access-Control-Allow-Headers",  "Origin,Content-Type,Accept,X-os,X-uid,X-token,X-role,X-Requested-With"); //用户代理是否应该在跨域请求的情况下从其他域发送cookies httpServletResponse.setHeader("Access-Control-Allow-Credentials", "true"); return true; } }
``````````

4.2、拦截器注入配置

``````````
@Configuration @EnableWebMvc @ComponentScan("com.recruit") public class WebAppConfig extends WebMvcConfigurerAdapter {  @Bean public AuthenticationInterceptor authenticationInterceptor() { return new AuthenticationInterceptor(); }  @Override public void addInterceptors(InterceptorRegistry registry) { //分页拦截器 registry.addInterceptor(new PaginationInterceptor()).addPathPatterns("/**"); //跨域拦截器 registry.addInterceptor(new CrossInterceptor()).addPathPatterns("/**"); //权限拦截器 registry.addInterceptor(authenticationInterceptor()).addPathPatterns("/**");  super.addInterceptors(registry); } }
``````````

![七、Spring Boot解决跨域问题][Spring Boot]


[Spring Boot]: /pro/os/crawler/FVIA-FIM6-BMBV.jpg
 *  **原文作者：** 程序员Nicky
 *  **原文链接：** https://www.toutiao.com/item/6570253010855789059/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。