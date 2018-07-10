---
title: 简述架构演变过程中对session存储以及权限校验的不同的解决方案
date: 2017-09-29 17:21:07
categories: "开发"
tags:
	- Google
	- NoSQL
	- Redis
	- 笔记本电脑
	- Mac

---

**session的作用**

这里说的session指的是用户浏览网站时所产生的会话，由于http协议是无状态的，服务器需要将这个会话和用户关联起来，这样才能知道每个请求是有哪个用户发出的，以做出相对应的处理和反馈。

**session的原理**

session的本质简单来理解可以当做是一个Map，你可以往里面放各种对象，如登录的用户，用户加入购物车的商品，等等和当前登入用户相关的一系列东西，而服务器对session的管理也可以理解是通过一个Map来维护，key是session\_id，value则是session，当每个浏览器访问服务器时，服务器都会生成一个唯一的session\_id告诉浏览器并存在cookie中，这样浏览器下次发起请求就会携带这个session\_id，这样服务器就能识别出当前发起请求的用户。

**单服务器环境下对session的管理**

在单机的web服务器环境下 不需要我们太对关注session管理和维护，因为服务器已经自身有了默认的解决方案，我们只需要直接通过相关的类获取到服务器的session即可

![简述架构演变过程中对session存储以及权限校验的不同的解决方案][session]

从图中可以看到，服务器内部是有一个session的大管家（SessionManager），主要作用就是管理session并且解析请求并把session放入当前请求的线程中。

随着系统请求量的增大，单服务器是完全满足不了现有业务的需求，因此服务器走向集群是必然的结果，带来了其中一个问题就是对session如果管理？

**服务器集群中的session处理**

**1、请求定向转发**

在集群中通常需要用到负载均衡器如Ngnix，如果采用轮循机制分发请求就可能导致同一个用户发送的两个请求分别被分配到了不同的机器上导致session不同步，所谓请求定向转发即一个用户的请求永远只会被分配到一台web服务器上，这样就能保证session的同步问题，但是这种方法有两个很大的弊端：1、负载均衡的效果很难体现，容易导致请求分配不均导致部分服务器压力过大；2、如果某一台服务器宕机将导致分配到这台用户的session全部失效。

![简述架构演变过程中对session存储以及权限校验的不同的解决方案][session 1]

**2、Session分发同步**

这种方案的原理是当一台服务器创建或者修改了session后，自动广播告知其他服务器，这种方案只适用于小的服务器集群，如果集群数量庞大容易带来集群风暴导致服务器性能严重降低！

![简述架构演变过程中对session存储以及权限校验的不同的解决方案][session 2]

**3、公共存储**

前面的解决方案都是将session任然放在本地服务器内存中，比较优雅的处理方案是直接存放于一个公共的位置中，如存放在Redis或者数据库（不推荐）中，通过替换掉服务器默认SessionManager来实现session存储到公共服务中。关于具体代码实现github上面有很多，基本拿来做些配置就能直接使用了

![简述架构演变过程中对session存储以及权限校验的不同的解决方案][session 3]

**4、令牌token**

关于令牌token的校验方式主要是在restful风格的api中使用，主要作用就是进行权限的校验，我自己总结了下token大体又可以分为3种类型

1、mac\_token

这种token一般用于不安全的网络下的 API 授权

数据格式如下：

\{

"user\_id":666666, // 用户标识

"access\_token":"2547TYSAs1zCsicMWpAA", // token 标识

"expires\_at":"2017-12-12T10:00:00Z", // 本 token 的过期时间

"refresh\_token:":"tGsg42OkF0XG5Qx2TlSGES", // 用以续期

"mac\_key":"adijq39jdlaska9asud", // hmac 的密钥

"mac\_algorithm","hmac-sha-256"// hmac 算法的名称

\}

Authorization http Header 在 client 发出需要进行 mac token 认证的 api 请求前，必须将 mac token 的信息放在 HTTP header 中的 Authorization 里面

Authorization:MAC id="2547TYSAs1zCsicMWpAA",nonce="1418752425123:dj83hs9s",mac="SLDJd4mg43cjQfElUs3Qub4L6xE="

具体加密的算法可以自己去定义，一般是将请求链接、参数、时间戳等数据组合在一起后加密，只要服务端和客户端的加密结果一致就认为是合法的token，token和用户信息的关联和session处理的原理差不多，可以通过redis进行存取

2、bearer token

\{

"user\_id":11232323, // 用户标识

"access\_token":"2YotnFZFEjr1zCsicMWpAA", // token 标识

"expires\_at":"2014-12-12T10:00:00Z", // 本 token 的过期时间（30天过期）

"refresh\_token:":"tGzv3JOkF0XG5Qx2TlKWIA", // 用以续期

\}

具体传输格式如：

Authorization: Bearer "2YotnFZFEjr1zCsicMWpAA"

与mac\_token的不同点在于bearer token一般是在服务器与服务器之间的api调用，是属于可靠传输因此不对token进行加密处理

3、JWT

与上面服务器响应的access\_token不同的地方在于，JWT中包含了一些用户的信息，比如用户id，用户名等，通过这种token进行用户校验有一个优点就是通过解析token就能得到用户的一些基本信息而不需要查询数据库，但是缺点就是增加了网络传输的数据量。其实我这样给token分类不是非常准确，可以吧JWT看做是另一种类型的access\_token，实际传输过程中可以看做只有mac或者bearer两种类型，jwt更具体的解析可以自行google查阅，抽空我会根据token的校验方式写一下具体的项目实践

![简述架构演变过程中对session存储以及权限校验的不同的解决方案][session 4]


[session]: /pro/os/crawler/AFEZ-UIEA-M2MF.jpg
[session 1]: /pro/os/crawler/VURY-VFIM-QMQU.jpg
[session 2]: /pro/os/crawler/REF7-BZU2-IR7R.jpg
[session 3]: /pro/os/crawler/VYQR-QYQE-IZFM.jpg
[session 4]: /pro/os/crawler/JZ7N-22JE-RRNQ.jpg
 *  **原文作者：** 疯狂的程序员
 *  **原文链接：** https://www.toutiao.com/item/6471127871174738446/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。