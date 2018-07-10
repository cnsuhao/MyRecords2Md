---
title: 阿里云Elasticsearch是什么样子？
date: 2017-12-14 23:23:35
categories: "开发"
tags:
	- Java
	- ElasticSearch
	- 编程语言
	- Lucene
	- 7-Eleven

---

# **如果觉得文章对你有用，请关注本头条号和微信公众号 - IT求职咨询** #

# **1. 概述** #

阿里云Elasticsearch提供开源Elasticsearch 5.5.3版本及商业版X-pack插件服务，致力于数据分析、数据搜索等场景服务。在开源Elasticsearch基础上提供企业级权限管控、安全监控告警、自动报表生成等功能。

Elasticsearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。

X-pack是Elasticsearch的一个商业版扩展包，将安全，警告，监视，图形和报告功能捆绑在一个易于安装的软件包中。它可以作为插件被快速集成在Kibana中，并提供给用户集群启用认证、角色权限管控、集群实时监控、可视化报表生成、机器学习等能力。

阿里云特点和优势

 *  分布式的实时文件存储，每个字段都被索引并可被搜索
 *  分布式的实时分析搜索引擎
 *  商业版X-pack插件，提供企业级权限管控、实时系统监控等强大服务
 *  弹性扩展到上百台服务器，处理PB级结构化或非结构化数据
 *  默认主流插件，包括第三方的IK分词插件等
 *  Elastic官方技术支持团队7\*24小时技术支持

**2. 预置插件（包含但不完全包含）**

ICU Analysis plugin ： lucene自带的ICU分词，ICU是一套稳定、成熟、功能强大、轻便易用和跨平台支持Unicode 的开发包

IK Analyzer：IK Analyzer是一个开源的，基于java语言开发的轻量级的中文分词工具包。是开源社区中处理中分分词非常热门的插件

Japanese (Kuromoji) Analysis plugin：日文分词器

Smart Chinese Analysis Plugin：lucene默认的中文分词器

Stempel (Polish) Analysis plugin：法文分词器

Mapper Attachments Type plugin：附件类型插件，通过tika库把各种类型的文件格式解析成字符串

**3. 访问方式**

仅支持Restful方式访问，Transport Client和Node Client方式不支持；Restful方式使用官方原生Elasticsearch Restful API，用户名为elastic，密码在申请时指定。通过Restful和Kibana访问，必须要填写用户名和密码，使用的是x-pack安全认证方式。

**4. 首页**

首页界面如图，

![阿里云Elasticsearch是什么样子？][Elasticsearch]

实例ID/名称是一个域名，版本信息显示为5.5.3，安装了x-pack，目前Elasticseach稳定版是5.6.3，说明阿里云并没有很及时更新，节点数为10，即有10台机器部署了Elasticsearch，规格代表了一台机器的配置 - CPU 1核、内存 2GB、硬盘 20GB SSD，SSD还是挺赞的，其他配置寒酸了点，毕竟收费产品，一小时3.78元，时间就是金钱。

**5. 基本信息**

基本信息如图，

![阿里云Elasticsearch是什么样子？][Elasticsearch 1]

基本信息部分右上角，可以进入Kibana控制台，也可以重启实例，这里应该是配置更改才会用到的操作。中间部分可以配置白名单和黑名单，利用的x-pack原生功能，kibana密码也可以重置；最下面部分yml配置也可以作修改。

在进入Kibana之后，观察集群默认配置：

``````````
GET _cluster/settings { "persistent": {}, "transient": { "cluster": { "routing": { "allocation": { "enable": "all" } } } } }
``````````

集群没有作额外设置，由业务自己设置。

``````````
GET _cat/indices green open .monitoring-es-6-2017.11.01 eXhV7qfdQRCsx8yuJLrRIw 1 1 4710 0 12.3mb 6.2mb green open .monitoring-kibana-6-2017.11.01 fSZ9oe3mTL6iBsmF8jF57A 1 1 264 0 626.2kb 306.4kb green open .monitoring-alerts-6 BWnuo8sNT_ecXbpiXCaRrw 1 1 0 0 324b 162b green open .kibana Spajg5vQSjSlIgFH2WiwVw 1 1 1 0 6.4kb 3.2kb green open .watcher-history-3-2017.11.01 bAHKX2FhS02zvRb6US-xUA 1 1 173 0 762.3kb 363.8kb green open .watches cmtN0kafRzG7U1zu6uiZTQ 1 1 4 0 136.8kb 68.4kb green open .triggered_watches 0nd7ikMkTa20qhZ24HvXow 1 1 0 0 34kb 17kb
``````````

索引列表也没有特别的，都是原生产生的。

``````````
GET _cat/nodes 10.18.94.5 15 71 0 0.01 0.03 0.05 mdi - HS3-5f3 10.18.94.8 18 72 7 0.17 0.20 0.20 mdi - lxW-87d 10.18.94.0 22 72 1 0.01 0.10 0.13 mdi - RvKor2n 10.18.93.254 13 71 0 0.01 0.04 0.07 mdi - zXtjkif 10.18.94.1 15 71 1 0.15 0.16 0.14 mdi - ZJu1hbs 10.18.94.6 14 71 1 0.00 0.03 0.07 mdi - sjOdSQC 10.18.94.7 16 71 1 0.00 0.01 0.05 mdi - _oBD4bR 10.18.93.253 13 72 1 0.01 0.05 0.05 mdi * puA1Xes 10.18.93.255 15 71 1 0.00 0.01 0.05 mdi - GM4Reqc 10.18.94.9 11 71 3 0.04 0.12 0.15 mdi - ZN_XpS-
``````````

从节点看，应该使用RPM安装，并没有采用master、data、client分离的方式部署。

![阿里云Elasticsearch是什么样子？][Elasticsearch 2]

Java端口默认开启的9300

**6. 总结**

1.  Elasticsearch和Kibana与原生保持一致
2.  仅支持Restful方式访问，不支持Java Transport方式访问
3.  支持x-pack安全认证等收费功能
4.  部署方式通过RPM，不支持master、client、data分离
5.  用户如果想使用好Elasticsearch和Kibana，更多还是要依赖对这两个产品的理解，阿里云本身没有提供优化


[Elasticsearch]: /pro/os/crawler/EZMR-AENE-ZQVI.jpg
[Elasticsearch 1]: /pro/os/crawler/EEUM-AI63-63MU.jpg
[Elasticsearch 2]: /pro/os/crawler/YQFY-7RB3-URZ2.jpg
 *  **原文作者：** IT求职咨询
 *  **原文链接：** https://www.toutiao.com/item/6494786566475481614/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。