---
title: 微服务——服务发现与消费、负载均衡（ribbon）、服务间通信
date: 2018-05-08 16:59:57
categories: "开发"
tags:
	- 技术
	- 通信

---

一、我们构建一个服务消费者，主要完成两个目标，发现服务以及消费服务。

1.服务的发现任务是由Eureka的客户端完成，在服务治理框架下，服务间的调用不再通过指定具体的实例地址来实现，而是通过向服务名发起请求调用实现，

所以服务调用方在调用服务提供方的时候，并不知道具体的服务实例的位置。因此，调用方需要向服务注册中心咨询服务，并获取所有服务的实例清单，以实现具体服务实例的访问。

发起调用时，从清单中以某中轮询策略取出一个位置进行服务调用，这就要客户端负载均衡。

2.服务消费的任务由Ribbon完成（完成了针对服务提供者的高可用）。Ribbon是一个基于HTTP和TCP的客户端负载均衡器，它可以通过客户端中配置的riboonServerList服务端列表

去轮询访问以达到均衡负载的作用。而且它不需要像注册中心、API网关那样独立部署，它可以存在于每一个springcloud构建的微服务中。

注：客户端负载均衡和服务端负载均衡的区别？

服务器端负载均衡：例如Nginx，通过Nginx进行负载均衡，通过维护一个可用的服务端清单，通过心跳检测来剔除故障的服务端节点以保证清单中都是可以正常访问的服务端节点。当客户发送请求到负载均衡设备的时候，

该设备按某种算法（比如线性轮询、按权重负载、按流量负载等）从维护的可用服务清单中取出一台服务端的地址，然后进行转发。

客户端负载均衡：例如spring cloud中的ribbon，最大的不同点在于上面提到的服务清单所在的位置，在客户端负载均衡中，所有客户端节点都维护着自己要访问的服务端清单，而这个清单来自于服务注册中心。

同服务端负载均衡的架构类似，在客户端负载均衡中也需要心跳去维护服务端清单的健康性。在发送请求前通过负载均衡算法选择一个服务器，然后进行访问，这是客户端负载均衡；

二、使用客户端负载均衡的方法：

1）.在服务消费者接入ribbon可以通过Eureka整合的方式，因为Eureka已经间接依赖于ribbon，所以不需要在pom中配置（当然也可以采用脱离Eureka的方式）

2） 服务提供者只需要启动多个服务实例并注册到注册中心

2）.服务消费者直接通过调用添加@LoadBalanced注解的RestTemplate来实现面向服务的接口调用

当ribbon和eureka联合使用的时候，ribbon的服务实例清单RibbonServerList会被重写，扩展成从Eureka注册中心获取服务列表。

三、RestTemplate详解

RestTemplate是spring内置的http请求封装，RestTemplate提供了多种便捷访问远程Http服务的方法。

1.首先构建两个RestTemplate，一个是直连的（测试用），一个是负载均衡。

![微服务——服务发现与消费、负载均衡（ribbon）、服务间通信][ribbon]

httpclient的properties和bean的构建：

![微服务——服务发现与消费、负载均衡（ribbon）、服务间通信][ribbon 1]

![微服务——服务发现与消费、负载均衡（ribbon）、服务间通信][ribbon 2]

2.自定义支持get和post方法的RestTemplate，并且支持直连和服务发现的调用。

![微服务——服务发现与消费、负载均衡（ribbon）、服务间通信][ribbon 3]

注意RestTemplate的exchage接口：

与其它接口的不同：

>允许调用者指定HTTP请求的方法（GET,POST,PUT等）

>可以在请求中增加body以及头信息，其内容通过参数‘HttpEntity<?>requestEntity’描述

>exchange支持‘含参数的类型’（即泛型类）作为返回类型，该特性通过‘ParameterizedTypeReference<T>responseType’描述。

# 3.服务的调用 #

> String url = "http://" + userServiceName + "/user/add";
> 
> ResponseEntity<RestResponse<User>> responseEntity = rest.post(url,account, new ParameterizedTypeReference<RestResponse<User>>() \{\});
> 
> RestResponse<User> response = responseEntity.getBody();
> 
> if (response.getCode() == 0) \{
> 
> return response.getResult();
> 
> \}\{
> 
> throw new IllegalStateException("Can not add user");
> 
> \}

调用RestTemplate的请求后返回的类型是RestEntity，该对象是Spring对HTTP请求响应的封装，主要存储HTTP的几个重要元素，

比如HTTP请求状态码HttpStatus，在它的父类HTTPEntity中还存储着HTTP请求头信息对象HttpHeader和请求体对象Http body。

我们再这里指定返回的body是RestResponse<T>泛型对象，而这需要ParameterizedTypeReference<T>进行描述。这里的T就是RestResponse<T>

> @JsonInclude(Include.NON\_NULL)
> 
> public class RestResponse<T> \{
> 
> private int code;
> 
> private String msg;
> 
> private T result;
> 
> public static <T> RestResponse<T> success() \{
> 
> return new RestResponse<T>();
> 
> \}
> 
> public static <T> RestResponse<T> success(T result) \{
> 
> RestResponse<T> response = new RestResponse<T>();
> 
> response.setResult(result);
> 
> return response;
> 
> \}
> 
> public static <T> RestResponse<T> error(RestCode code) \{
> 
> return new RestResponse<T>(code.code,code.msg);
> 
> \}
> 
> public RestResponse()\{
> 
> this(RestCode.OK.code, RestCode.OK.msg);
> 
> \}
> 
> public RestResponse(int code,String msg)\{
> 
> this.code = code;
> 
> this.msg = msg;
> 
> \}
> 
> public int getCode() \{
> 
> return code;
> 
> \}
> 
> public void setCode(int code) \{
> 
> this.code = code;
> 
> \}
> 
> public String getMsg() \{
> 
> return msg;
> 
> \}
> 
> public void setMsg(String msg) \{
> 
> this.msg = msg;
> 
> \}
> 
> public T getResult() \{
> 
> return result;
> 
> \}
> 
> public void setResult(T result) \{
> 
> this.result = result;
> 
> \}
> 
> @Override
> 
> public String toString() \{
> 
> return "RestResponse \[code=" + code + ", msg=" + msg + ", result=" + result + "\]";
> 
> \}
> 
> \}

所有的返回对象都是这个RestResponse<T>对象，response.getCode()根据获得的状态码判断是否正确返回，如果发生错误则抛出错误。


[ribbon]: /pro/os/crawler/MQ22-UNAE-NF6J.jpg
[ribbon 1]: /pro/os/crawler/Q7RY-VME7-VZMB.jpg
[ribbon 2]: /pro/os/crawler/FFFN-UBAV-VVUR.jpg
[ribbon 3]: /pro/os/crawler/QFR7-ZBZM-R2UM.jpg
 *  **原文作者：** JAVA小生
 *  **原文链接：** https://www.toutiao.com/item/6553132239184462339/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。