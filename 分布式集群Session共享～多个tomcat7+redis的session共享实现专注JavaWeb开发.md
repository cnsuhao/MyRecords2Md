---
title: 分布式集群Session共享～多个tomcat7+redis的session共享实现
date: 2017-10-04 12:39:34
categories: "开发"
tags:
	- Java
	- Tomcat
	- NoSQL
	- Redis
	- HTML

---

**什么是Session/Cookie**

用户使用网站的服务，基本上需要浏览器与Web服务器的多次交互。HTTP协议本身是无状态的，当用户的第一次访问请求结束后，后端服务器就无法知道下一次来访问的还是不是上次访问的用户。我们需要基于HTTP协议支持会话状态的机制，这样的机制可以使Web服务器从多次单独的HTTP请求中知道哪些请求是来自哪个会话的。

Session与Cookie的作用都是为了保持访问用户与后端服务器的交互状态。

理解Cookie

Cookie的作用通俗的说就是当一个用户通过HTTP协议访问一个服务器的时候，这个服务器会将一些Key/Value键值对返回给客户端浏览器，并给这些数据加上一些限制条件，在条件符合时这个用户下次访问这个服务器的时候，数据又被完整地带回给服务器。

理解Session

Cookie可以让服务端程序跟踪每个客户端的访问，但是每次客户端的访问都必须传回这些Cookie，如果Cookie很多，这无形地增加了客户端与服务端的数据传输量，而Session的出现正是为了解决这个问题。

同一个客户端每次和服务端交互时，不需要每次都传回所有的Cookie值，而是只要传回一个会话标识（SessionId），这个ID是客户端第一次访问服务器的时候生成的，而且每个客户端是唯一的。这样每个客户端就有一个唯一的ID，客户端只要传回这个ID就行了，这个ID通常是NAME为JSESIONID的一个Cookie。在Web服务器上，各个会话独立存储保存不同会话的信息。如果遇到禁用Cookie的情况，一般的做法就是把这个会话标识放到URL的参数中。

总结

Cookie和Session都是为了保持用户访问的连续状态，之所以要保持这种状态，一方面是为了方便业务实现，另一方面就是简化服务端程序设计，提高访问性能。但也带来了一些问题，如安全问题、集群环境下Session同步问题等

集群遇到的问题

当我们的应用服务器变为多台的时候就会遇到Session共享的问题。当我们第一次访问网站的时候，负载均衡将本地的请求分配到Web服务器1，那么Session创建在Web服务器1，第二次访问的时候如果我们不做处理就不能保证还是会落到Web服务器1了。

解决集群Session共享问题

1. Session Sticky

让负载均衡器能够根据每次的请求的会话标识来进行请求的转发，这样就能保证每次都能落到同一台服务器上面，这种方式称为Session Sticky方式。如下图：

![分布式集群Session共享～多个tomcat7+redis的session共享实现][Session_tomcat7_redis_session]

存在问题：

1. 如果这一台Web服务器宕机或者重启了，服务器上的会话数据会丢失，用户需要重新登陆等。

2. 会话标识是应用层的信息，那么负载均衡器要将同一个会话的请求都保存到同一个Web服务器上的话，就需要进行应用层的解析，这个开销比第四层交换（LVS负载均衡器属于第四层）要大。

3. 负载均衡器变为一个有状态的节点，要将会话保存到具体的Web服务器的映射。和无状态的节点相比，内存消耗会更大，容灾方面会更麻烦。

打个比方，比如Web服务器是饭店，会话数据是碗筷。要保证每次吃饭都用自己的碗筷的话，我就把餐具存在某一家饭店，并且每次都去这家店吃饭。

2. Session Replication

还是以吃饭的例子，如果我们在每个饭店都存放一套自己的碗筷，就可以自己的选择去哪家吃饭了。这就是Session Replication。如下图：

![分布式集群Session共享～多个tomcat7+redis的session共享实现][Session_tomcat7_redis_session 1]

此方案不用再要求负载均衡器保证同一个会话的多次请求必须到同一个Web服务器上了。我们在Web服务器之间增加了会话数据的同步，通过同步就保证了不同Web服务器之间Session数据的一致。一般应用容器都支持Session Replication方式，与Session Sticky方案相比，Session Replication方式对负载均衡器没有那么多的要求。

存在问题：

1. 同步Session数据造成了网络带宽的开销。只要Session数据有变化，就需要将数据同步到所有其他机器上，机器越多，同步带来的网络带宽开销就越大。

2. 每台Web服务器都要保存所有Session数据，如果整个集群的Session数据很多（很多人同时访问网站）的话，每台机器用于保存Session数据的内容占用会很严重。

这个方案是靠应用容器来完成Session的复制从而解决Session的问题的，应用本身并不关心这个事情。这个方案不适合集群机器数多的场景。如果只有几台机器，用这个方案是可以的。

3. Session数据集中存储

将Session数据集中存储，然后不同Web服务器从同样的地方获取Session，如下图：

![分布式集群Session共享～多个tomcat7+redis的session共享实现][Session_tomcat7_redis_session 2]

Session数据不保存到本机而且存放到一个集中存储的地方，修改Session也是发生在集中存储的地方。Web服务器使用Session从集中存储的地方读取。这样保证了不同Web服务器读取到的Session数据都是一样的。存储Session的具体方式可以是数据库、分布式存储系统等。这个方案解决了Session Replication方案中内存的问题，对于网络带宽也比Session Replication要好。

存在问题：

1. 读写Session数据引入了网络操作，这相对于本机的数据读取来说，问题就在于存在时延和不稳定性，不过我们的通讯基本都是发生在内网，问题不大。

2. 如果集中存储Session的机器或者集群有问题，就会影响到我们的应用。

相对于Session Replication，当Web服务器数量比较大、Session数比较多的时候，这个集中存储方案的优势是非常明显的。

4. Cookie Based

这个方案对于同一个会话的不同请求也是不限制具体处理机器的。它是通过Cookie来传递Session数据的，如下图：

![分布式集群Session共享～多个tomcat7+redis的session共享实现][Session_tomcat7_redis_session 3]

从上图可以看出，我们的Session数据放到Cookie中，然后在Web服务器上从Cookie中生成对应的Session数据。这就好比我们每次都把自己的碗筷带在身上，这样去那家饭店就可以随意选择了。相对前面的集中存储方案，不会依赖外部的存储系统，也就不存在从外部系统获取、写入Session数据的网络时延、不稳定性了。

存在问题：

1. Cookie长度的限制。我们知道Cookie是有长度限制的，而这也会限制Session数据的长度。

2. 安全性。Session数据本来都是服务端数据，而这个方案是让这些服务端数据到了外部网络及客户端，因此存在安全性上的问题。我们可以对写入的Cookie的Session数据做加密，不过对于安全来说，物理上不能接触才是安全的。

3. 带宽消耗。指的是我们数据中心的整体外部带宽的消耗。

4. 性能影响。每次HTTP请求和响应都带有Session数据，对Web服务器来说，在同样的处理情况下，响应的结果输出越少，支持的并发请求就越多。生成Session数据也会影响处理速度。

这4个方案都是可用的方案，但是对于大型网站来说，Session Stick和Session数据集中存储是比较好的方案，这两个方案各有优劣，需要在具体的场景中做出选择和权衡。

以下我们对第三种方案Session数据集中存储进行简单的例子介绍

简单tomcat8+redis的session共享实现

1. 下载开源项目

https://github.com/jcoleman/tomcat-redis-session-manager

2. 创建maven项目

创建项目并把src/main/java/com/orangefunction/tomcat/redissessions/复制到项目

3. 支持tomcat8需修改代码

RedisSessionManager.java

<table>
 <tbody>
  <tr>
   <td></td>
   <td>@@ -713,9 +713,9 @@ private void initializeSerializer() throws ClassNotFoundException, IllegalAccess</td>
   <td></td>
   <td></td>
  </tr>
  <tr>
   <td></td>
   <td>serializer = (Serializer) Class.forName(serializationStrategyClass).newInstance();</td>
   <td></td>
   <td>serializer = (Serializer) Class.forName(serializationStrategyClass).newInstance();</td>
  </tr>
  <tr>
   <td></td>
   <td></td>
   <td></td>
   <td></td>
  </tr>
  <tr>
   <td></td>
   <td>Loader loader =null;</td>
   <td></td>
   <td>Loader loader =null;</td>
  </tr>
  <tr>
   <td></td>
   <td>-</td>
   <td></td>
   <td>+Context context =this.getContext();</td>
  </tr>
  <tr>
   <td></td>
   <td>- if (getContainer()!=null) {</td>
   <td></td>
   <td>+ if (context!=null) {</td>
  </tr>
  <tr>
   <td></td>
   <td>- loader =getContainer().getLoader();</td>
   <td></td>
   <td>+ loader =context.getLoader();</td>
  </tr>
  <tr>
   <td></td>
   <td>}</td>
   <td></td>
   <td>}</td>
  </tr>
  <tr>
   <td></td>
   <td></td>
   <td></td>
   <td></td>
  </tr>
  <tr>
   <td></td>
   <td>ClassLoader classLoader =null;</td>
   <td></td>
   <td>ClassLoader classLoader =null;</td>
  </tr>
 </tbody>
</table>

4. 打包部署

将实现包和依赖包commons-pool2-2.3.jar、jedis-2.7.2.jar、tomcat8\_redis\_session-0.0.1-SNAPSHOT.jar拷贝到tomcat的lib下

新增tomcat context.xml配置

<Valve className="com.demo.redis\_session.RedisSessionHandlerValve" />

<Manager className="com.demo.redis\_session.RedisSessionManager"

host="127.0.0.1"

port="6379"

database="0"

maxInactiveInterval="60" />

资料在项目的data目录下

5. 测试

在web项目启动tomcat并使用如下代码进行测试

publicvoiddoGet(HttpServletRequestrequest,HttpServletResponseresponse)throwsServletException,IOException\{HttpSessionsession=request.getSession();/\*//1.设置Sessionfor(inti=0;i<10;i++)\{session.setAttribute("name"+i,"session\_data\_"+i);\}\*///2.注释第一步,重启tomcat后看是否还可以读取到SessionStringstr="";for(inti=0;i<10;i++)\{str=str+session.getAttribute("name"+i)+"<br>";\}response.setContentType("text/html");PrintWriterout=response.getWriter();out.println(str);out.flush();out.close();//访问:http://localhost:8080/tomcat8\_redis\_session\_web/servlet/TestRedisSessionServlet\}

--------------------

参考网址：

redis + Tomcat 8 的session共享解决 【http://www.cnblogs.com/interdrp/p/4868740.html 】

Tomcat7+Redis存储Session【http://blog.csdn.net/caiwenfeng\_for\_23/article/details/45666831】

用Redis存储Tomcat集群的Session【http://blog.csdn.net/chszs/article/details/42610365 】

分布式集群系统下的高可用session解决方案【http://tendyming.iteye.com/blog/1815136 】

《大型网站系统与Java中间件实践》

《深入分析Java Web技术内幕》

code：

https://github.com/JeromeSuz/tomcat8\_redis\_session

https://github.com/JeromeSuz/tomcat8\_redis\_session\_web

**好了，今天的技术内容就分享给大家，码农不容易，小编给大家分享写博客也不容易，请多多支持，喜欢请关注头条号，每天都有功能和bug分享给大家一起学习进步，我们的目标是 ----软件攻城狮**

**码农不容易，码文章更不容易啊，喜欢小编多多支持请点击关注哦，小编会更加努力每天给大家分享技术文章。**


[Session_tomcat7_redis_session]: /pro/os/crawler/QIIF-AAY6-BEFZ.jpg
[Session_tomcat7_redis_session 1]: /pro/os/crawler/7Z6F-RUVB-BJRI.jpg
[Session_tomcat7_redis_session 2]: /pro/os/crawler/7ZAE-EIU6-BBVJ.jpg
[Session_tomcat7_redis_session 3]: /pro/os/crawler/V32A-JEN6-VUVU.jpg
 *  **原文作者：** 专注JavaWeb开发
 *  **原文链接：** https://www.toutiao.com/item/6472910208862519821/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。