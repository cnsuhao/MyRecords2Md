---
title: 基于微服务框架单点登录开发
date: 2018-02-02 17:39:19
categories: "开发"
tags:
	- 技术
	- BASIC语言

---

# 单点登录概念 #

单点登录（Single Sign On），简称为 SSO，是目前比较流行的企业业务整合的解决方案之一。SSO的定义是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。登录逻辑如上图

# 基于Spring 全家桶的实现 #

技术选型：

![基于微服务框架单点登录开发][YEYQ-E32Y-R6NB.jpg]

.

客户端：

maven依赖

![基于微服务框架单点登录开发][URUJ-UBFB-J73A.jpg]

.

EnableOAuth2Sso 注解

入口类配置@@EnableOAuth2Sso

![基于微服务框架单点登录开发][ZNAF-6FYI-M322.jpg]

.

配置文件

![基于微服务框架单点登录开发][FRQF-FQVZ-7FYV.jpg]

.

# SSO认证服务器 #

认证服务器配置

![基于微服务框架单点登录开发][FV7B-RRMZ-BYUM.jpg]

.

![基于微服务框架单点登录开发][QN7V-VRA6-BQEV.jpg]

.

# 配置完成体验 #

1.  访问SSO客户端的 index.html
2.  重定向到SSO服务端的 Basic 认证
3.  输入账号密码又重定向到原请求的 客户端index资源

# 总结 #

 *  客户端访问服务端 403问题？ 用户需要拥有ROLE\_USER的权限，具体的可以通过日志可以查看到报错。
 *  Possible CSRF detected - state parameter was present but no state could be found 目前是通过设置session： never或者 cotext-path解决
 *  源码请参考 gitee.com/log4j/ 基于Spring Cloud、Spring Security Oauth2.0开发企业级认证与授权，提供常见服务监控、链路追踪、日志分析、缓存管理、任务调度等实现

作者：冷冷gg


[YEYQ-E32Y-R6NB.jpg]: /pro/os/crawler/YEYQ-E32Y-R6NB.jpg
[URUJ-UBFB-J73A.jpg]: /pro/os/crawler/URUJ-UBFB-J73A.jpg
[ZNAF-6FYI-M322.jpg]: /pro/os/crawler/ZNAF-6FYI-M322.jpg
[FRQF-FQVZ-7FYV.jpg]: /pro/os/crawler/FRQF-FQVZ-7FYV.jpg
[FV7B-RRMZ-BYUM.jpg]: /pro/os/crawler/FV7B-RRMZ-BYUM.jpg
[QN7V-VRA6-BQEV.jpg]: /pro/os/crawler/QN7V-VRA6-BQEV.jpg
 *  **原文作者：** 一个小小的java码农
 *  **原文链接：** https://www.toutiao.com/item/6517889291354391043/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。