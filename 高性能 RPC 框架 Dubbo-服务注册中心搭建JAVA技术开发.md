---
title: 高性能 RPC 框架 Dubbo-服务注册中心搭建
date: 2018-05-03 16:01:00
categories: "开发"
tags:
	- Java虚拟机
	- 技术
	- Hadoop
	- Apache

---

整体来说，一个公司业务系统的演进流程基本都是从单体应用到多应用。在单体应用时，不同业务模块相互调用直接在本地 JVM 进程内就可以完成，而变为多个应用时，相互之间进行通信的方式就不能简单的进行本地调用了，因为不同业务模块部署到了不同的 JVM 进程里面，更常见的是部署到了不同的机器，这时候一个高效、稳定的 RPC 远程调用框架就变得非常重要。

Dubbo 是阿里巴巴开发的一个开源的高性能的远程服务调用框架，致力于提供高性能和透明化的 RPC 远程调用服务解决方案。作为阿里巴巴 SOA 服务化治理方案的核心框架，目前它已进入 Apache 卵化器项目，其前景可谓无限光明。

#  #

二、使用ZooKeeper搭建服务注册中心

2.1 ZooKeeper基本原理

Zookeeper 是 Apacahe Hadoop 的子项目，是一个树型的目录服务，支持变更推送，适合作为 Dubbo 服务的注册中心，工业强度较高，推荐生成环境使用。

![高性能 RPC 框架 Dubbo-服务注册中心搭建][RPC _ Dubbo-]

下面结合上图介绍Zookeeper在服务注册与发现里面的应用：

 *  如上图整体zk的树根Root是dubbo，说明建立的zk分组为dubbo，树的第二层为Service层用来表示具体的接口服务这里为com.test.UserServiceBo接口服务，树的第三层为Type层用来区分是服务提供者、还是服务消费者、还是路由规则等，树的第四层URl是具体服务提供者或者消费者对应的机器列表或者具体的路由规则。
 *  假设服务提供者对外提供com.test.UserServiceBo的实现类的服务，那么当服务提供者启动时候，Provider会通过zkclient在zk服务端的/dubbo/com.test.UserServiceBo/providers目录下写入自己的URL地址，如果服务提供者有多个，那么providers下就会有多个地址，这里需要注意的是这里的树形目录不一定是二叉树，假如服务提供者有100个机器，那么providers下就会有100个节点的。
 *  假设服务消费者需要使用com.test.UserServiceBo接口的服务，那么当服务消费者启动时候，Consumer会通过zkclient订阅zk服务端的/dubbo/com.test.UserServiceBo/providers目录下提供的服务提供方的URL地址列表。并且在/dubbo/com.test.UserServiceBo/consumers目录下写下自己的url地址，这里要记录服务消费者的地址是为了当服务提供者地址列表变化时候（比如新增了机器，减少了机器）时候zk可以通知订阅该服务的订阅者。
 *  监控中心和管理控制台在启动时候会通过zkclient订阅zk服务端的/dubbo/com.test.UserServiceBo/目录下的内容，并通过providers和consumers子目录区分哪些是服务提供者列表，哪些是服务消费者列表，另外管理控制台还可以写入路由规则到/dubbo/com.test.UserServiceBo/routers目录,这些路由规则也会被推送到服务消费端。

当服务提供者集群中有一台机器挂了后，zk能及时通过长链断开发现该机器挂了，并会从zk注册中心服务提供者的providers目录下删除该机器，然后会通知服务消费者更新可用的服务提供者的地址列表 由于zk支持持久化存储，所以当zk重启后，可以自动恢复服务注册信息。

2.2 ZooKeeper的安装

本文我们讲解使用 Apache ZooKeeper 作为服务注册中心时候ZooKeeper的搭建。

 *  首先你需要到 http://zookeeper.apache.org/releases.html 下载一个 zk 的包，本文作者使用的是 zookeeper-3.4.11 这个版本，如下图：
 *  然后修改 zookeeper-3.4.11/conf 文件夹里面的 zoo.cfg 文件。
    
    设置配置项 dataDir 为一个存在的以 data 结尾的目录；
    
    设置 zk 的监听端口 clientPort=2181；
    
    设置 zk 心跳检查间隔 tickTime = 2000;
    
    设置 Follower 服务器启动时候从 Leader 同步完毕数据能忍受多少个心跳时间间隔数 initLimit=5。
    
    设置运行过程中 Leader 同步数据到 Follower 后，Follower 回复信息到 Leader 的超时时间 syncLimit=2，如果 Leader 超过 syncLimit 个 tickTime 的时间长度，还没有收到 Follower 响应，那么就认为这个 Follower 已经不在线了：
 *  最后在 zookeeper-3.4.11/bin 下运行 sh zkServer.sh start-foreground 就会启动 zk，会有下面输出：

![高性能 RPC 框架 Dubbo-服务注册中心搭建][RPC _ Dubbo- 1]

 *  解压该包后，如下图：

![高性能 RPC 框架 Dubbo-服务注册中心搭建][RPC _ Dubbo- 2]

 *  

![高性能 RPC 框架 Dubbo-服务注册中心搭建][RPC _ Dubbo- 3]

 *  

![高性能 RPC 框架 Dubbo-服务注册中心搭建][RPC _ Dubbo- 4]

 *  

可知 zk 在端口 2181 进行监听，至此服务注册中心搭建完毕。

2.3 ZooKeeper的使用

服务提供方和调用方需要引入zkclient的jar包才能使用访问zk服务器，需要在pom里面添加下面依赖、

``````````
<dependency> <groupId>com.101tec</groupId> <artifactId>zkclient</artifactId> <version>0.10</version></dependency>
``````````

Zookeeper的单机配置:指定一个zk的ip作为服务注册中心

<dubbo:registry address="zookeeper://12.22.123.101:2181" /> or

<dubbo:registry protocol="zookeeper" address="12.22.123.101:2181" />

其中zookeeper指明使用zookeeper作为服务注册中心，12.22.123.101:2181是zk的服务器地址和服务监听端口号。

Zookeeper 集群配置：指定多个ip作为服务注册中心

<dubbo:registry address="zookeeper://11.10.13.10:2181?backup=11.20.153.111:2181,11.30.

133.112:2181" /> or

<dubbo:registry protocol="zookeeper" address="11.10.13.10:2181,11.20.153.111:2181,11.30.

133.112:2181" />

另外你还可以在同一个zk服务器上划分多个分组，例如下面：

<dubbo:registry id="registry1" protocol="zookeeper" address="10.20.153.10:2181" gr

oup="registry1" />

<dubbo:registry id="registry2" protocol="zookeeper" address="10.20.153.10:2181" gro

up="registry2" />

如上代码，在同一个zk服务器上划分了两个分组，也就是会有两颗树目录，树根分别为registry1和registry2.


[RPC _ Dubbo-]: http://p1.pstatp.com/large/pgc-image/1525334232910c65130acbc
[RPC _ Dubbo- 1]: http://p3.pstatp.com/large/pgc-image/1525334255032af614b8e27
[RPC _ Dubbo- 2]: http://p1.pstatp.com/large/pgc-image/1525334263792245a1e93c4
[RPC _ Dubbo- 3]: http://p9.pstatp.com/large/pgc-image/15253342862693721dd164e
[RPC _ Dubbo- 4]: /pro/os/crawler/JAQI-U3UY-JVNZ.jpg
 *  **原文作者：** JAVA技术开发
 *  **原文链接：** https://www.toutiao.com/item/6551261623967810055/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。