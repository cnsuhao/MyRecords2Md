---
title: 国产开源项目Cola Cloud：构建微服务架构企业级开发平台
date: 2018-04-01 15:25:55
categories: "开发"
tags:
	- 科技
	- UC浏览器

---

今天老猫要跟大家分享一个开源项目Cola Cloud 基于 Spring Boot, Spring Cloud 构建微服务架构企业级开发平台，开发者：leecho ，代码可以到码云下载。搜索项目名 Cola Cloud。

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud]

# 简介： #

Cola Cloud 基于 Spring Boot, Spring Cloud 构建微服务架构企业级开发平台，集成OAuth2认证、集成短信验证码登录、FlyWay数据库版本管理、网关集成Swagger聚合所有服务API文档。基于SpringBootAdmin集成Hystrix、Turbine监控。开发用户中心、权限管理、组织架构、数据字典、消息中心、通知中心等模块。

# 服务介绍 #

    | 项目名称                    | 编号                   | 名称   | 说明                                                                    |
    | ----------------------- | -------------------- | ---- | --------------------------------------------------------------------- |
    | cola-cloud-auth         | auth-service         | 认证服务 | 基于SpringSecurity进行安全认证，采用OAuth2.0认证体系，对客户端、用户进行认证及授权，支持账号密码登录，短信验证码登录 |
    | cola-cloud-config       | config               | 配置服务 | 基于Spring Cloud构建统一配置服务，负责管理所有服务的配置文件                                  |
    | cola-cloud-devtools     | 无                    | 开发工具 | Cola 代码生成器                                                            |
    | cola-cloud-gateway      | gateway              | 服务网关 | 基于Zuul构建服务网关，并对服务进行负载，前只实现静态路由                                        |
    | cola-cloud-monitor      | monitor              | 服务监控 | 基于Spring Boot Admin集成Turbine,Hystrix，对应用状态进行监控，对服务调用进行追踪和对熔断进行监测      |
    | cola-cloud-message      | message              | 通知中心 | 公共基础通知服务，支持系统消息、短信、邮件、推送通知                                            |
    | cola-cloud-registry     | registry             | 服务注册 | 基于Euraka构建服务注册中心，负责服务注册于发现                                            |
    | cola-cloud-common       | common-service       | 基础服务 | 聚合Cola平台所有公共服务                                                        |
    | cola-cloud-organization | organization-service | 组织架构 | 提供组织架构、员工、岗位等服务                                                       |
    | cola-cloud-tenancy      | tenancy-service      | 租户服务 | 提供租户以及租户成员服务                                                          |
    | cola-cloud-uc           | uc-service           | 租户服务 | 用户中心                                                                  |
    | cola-cloud-upm          | upm-service          | 权限服务 | 提供角色、资源、授权服务                                                          |
    | cola-cloud-notification | notification-service | 通知中心 | 基于RabbitMQ异步通知发送短信、邮件、WebSocket消息                                     |

# 快速启动 #

**配置HOST**

Spring Cloud中的每个服务都是独立部署，所有在进行服务之间调用的时候需要确定对方服务的IP，为了规避IP变化带来代码修改的风险，所以需要配置host

``````````
127.0.0.1 registry config monitor auth-service uc-service upm-service organization-serivce tenancy-service
``````````

**启动服务**

启动顺序如下：`config registry auth-service uc-serivce upm-service organization-service gateway monitor`

config必须要最先启动，因为其负责提供给其他服务配置信息，如果config没有启动，其他服务则无法启动

registry在config之后启动，registry启动之后提供接口以供其他服务进行注册

其他service在registry之后启动，如果是第一次运行项目，启动registry之后先启动uc-service进行数据初始化

gateway在最后启动，如果gateway先于其他服务启动，可能无法代理到其他服务，不过会在一段时间后重新代理

monitor，在config启动之后即可启动

**访问**

``````````
http://localhost:4000/ 服务网关，已经聚合了所有服务的Swaggerhttp://localhost:8761/ 注册中心，可以查看服务注册情况http://localhost:8080/ 监控中心，可以查看服务运行状态
``````````

**获取ACCESS\_TOKEN**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 1]

# 系统截图 #

**获取Token**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 2]

**注册中心**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 3]

**API文档**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 4]

**监控中心**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 5]

**监控详细信息**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 6]

**链路追踪**

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 7]

# 开源协议 #

MIT


# 项目点评 #

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 8]

> 欢迎关注老猫码坊，与老猫一起了解更多互联网科技和编程知识。

![国产开源项目Cola Cloud：构建微服务架构企业级开发平台][Cola Cloud 9]


[Cola Cloud]: /pro/os/crawler/VZER-B3IZ-RR7N.jpg
[Cola Cloud 1]: /pro/os/crawler/VUMF-6RMB-FIZI.jpg
[Cola Cloud 2]: /pro/os/crawler/REFQ-REFQ-7FFY.jpg
[Cola Cloud 3]: /pro/os/crawler/YNMA-YNQJ-EFEV.jpg
[Cola Cloud 4]: /pro/os/crawler/ZAJQ-I2NY-IJBA.jpg
[Cola Cloud 5]: /pro/os/crawler/MMRZ-7FMU-J77J.jpg
[Cola Cloud 6]: /pro/os/crawler/ZMJV-B3IY-R6BM.jpg
[Cola Cloud 7]: /pro/os/crawler/BIQV-RIZJ-UJE2.jpg
[Cola Cloud 8]: http://p9.pstatp.com/large/pgc-image/1522567208829f5bfeb4200
[Cola Cloud 9]: /pro/os/crawler/JNMZ-RJFU-VREA.jpg
 *  **原文作者：** 老猫码坊
 *  **原文链接：** https://www.toutiao.com/item/6539377856550535683/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。