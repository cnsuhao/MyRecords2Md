---
title: java基于RabbitMQ+Hessian+spring实现RPC远程调用框架
date: 2017-10-28 21:02:52
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言

---

![java基于RabbitMQ+Hessian+spring实现RPC远程调用框架][java_RabbitMQ_Hessian_spring_RPC]

**1.对Rpc的简单阐述** 

对RPC通俗的理解就是，调用远程服务和调用本地服务一样透明化无感知。使用过dubbo和motan的同学肯定有这种感觉。实现RPC调用过程，无非解决两个问题：

1.数据的传输：这里使用RabbitMQ来收发消息，保证消息的可靠性

2.请求和响应数据的序列化和反序列化：采用Hessian

如果有自己的序列化方案，还得确定传输的消息体结构，这里不做考虑

**2.远程调用过程**

首先：消费者和生产者spring容器初始化的时候，会根据配置的的api在RabbitMQ上建立相应的队列,消费者会监听相关队列

1）生产者（client）调用以本地调用方式调用服务；

2）client 接收到调用后通过Hessian将方法、参数等组装成能够进行网络传输的消息体；

3）client 通过代理类，执行invoke方法，统一将消息发送到MQ监听的服务端；

4）server 收到消息后通过Hessian进行解码；

5）server 根据解码结果调用本地的服务；

6）本地服务执行并将结果返回给server ；

7）server 将返回结果通过Hessian打包发送至消费方；

8）client 接收到消息，并进行解码；

9）生产者得到最终结果。

**3.具体实现过程（如下类图）**

![java基于RabbitMQ+Hessian+spring实现RPC远程调用框架][java_RabbitMQ_Hessian_spring_RPC 1]

简述：MQClientProxy是继承InvocationHandler的代理类，MQClientProxyFactoryBean实现了FactoryBean接口，在spring初始化bean的时候，由MQClientProxy类提供代理类，所有的接口方法调用，最终都会执行MQClientProxy的invoke方法，只是每个接口传过来的的Invoke实参会不一样，然后内容经过Hessian打包后，由RabbitTemplate发送到队列。MQServerEndpoint在服务端容器初始化的时候会根据接口名称监听相应的队列，收到消息后经由Hessian的HessianSkeleton的方法invoke执行后返回

**4.源码地址**

我的coding地址：https://gitee.com/kailing/springboot-mqrpc

spring boot版地址：https://gitee.com/kailing/springboot-mqrpc

市场上成熟的RPC框架有很多，比如Dubbo，Motan，Thrift等，写这篇博文只为加深对RPC原理的认识，有兴趣的可以直接看源码


[java_RabbitMQ_Hessian_spring_RPC]: /pro/os/crawler/YBJQ-FAFI-63AM.jpg
[java_RabbitMQ_Hessian_spring_RPC 1]: /pro/os/crawler/QIQJ-3MFR-FQEZ.jpg
 *  **原文作者：** 小陈博主
 *  **原文链接：** https://www.toutiao.com/item/6481946482831278606/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。