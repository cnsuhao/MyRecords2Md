---
title: Nginx负载均衡与Redis实现Tomcat的session共享基本配置
date: 2018-04-18 10:31:42
categories: "开发"
tags:
	- Nginx
	- Tomcat
	- NoSQL
	- Redis
	- HTML

---

1、Nginx简介

根据Nginx官网说明可知：nginx \[engine x\]是最初由Igor Sysoev编写的HTTP和反向代理服务器，邮件代理服务器和通用TCP / UDP代理服务器。

2、Nginx负载均衡配置

a) nginx.conf是nginx的配置文件，在conf目录下边，其中\#开头的都是注释。

b) 主要需要配置的是http模块下的内容

![Nginx负载均衡与Redis实现Tomcat的session共享基本配置][Nginx_Redis_Tomcat_session]

![Nginx负载均衡与Redis实现Tomcat的session共享基本配置][Nginx_Redis_Tomcat_session 1]

> 红框所标是转发的服务器地址，在项目中也就是部署的tomcat地址，只需配置ip和端口号即可。
> 
> 配置nginx监听端口和转发的upstream。
> 
> 1.  upstream mynginxserver\{
> 2.  
> 3.  server172.16.10.228:8053 weight=5;
> 4.  
> 5.  server 172.16.10.225:8050;
> 6.  \}

配置负载均衡还要注意以下几点

> 1.  1.$remote\_addr 与$http\_x\_forwarded\_for 用以记录客户端的ip地址；
> 2.  2.$remote\_user ：用来记录客户端用户名称；
> 3.  3.$time\_local ： 用来记录访问时间与时区；
> 4.  4.$request ： 用来记录请求的url与http协议；
> 5.  5.$status ： 用来记录请求状态；成功是200，
> 6.  6.$body\_bytes\_s ent ：记录发送给客户端文件主体内容大小；
> 7.  7.$http\_referer ：用来记录从那个页面链接访问过来的；
> 8.  8.$http\_user\_agent ：记录客户端浏览器的相关信息；
> 9.  **\[html\]** view plain copy
> 10. proxy\_set\_headerHost $host:$server\_port;
> 11. 
> 12. proxy\_set\_headerX-Real-IP $remote\_addr;
> 13. 
> 14. proxy\_set\_header REMOTE-HOST $remote\_addr;
> 15. 
> 16. proxy\_set\_header X-Forwarded-For$proxy\_add\_x\_forwarded\_for;
> 17. 
> 18. proxy\_redirect off;
> 19. 
> 20. proxy\_pass\_header Set-Cookie;
> 21. 
> 22. proxy\_hide\_header X-Powered-By;
> 23. 
> 24. proxy\_hide\_header X-Mod-Pagespeed;
> 25. 
> 26. proxy\_ignore\_client\_abort off;
> 27. 
> 28. proxy\_cache\_valid any 10m;
> 29. 
> 30. proxy\_connect\_timeout 75s;
> 31. 
> 32. proxy\_read\_timeout 75s;
> 33. 
> 34. proxy\_send\_timeout 75s;

c) 启动nginx,命令台进入nginx目录，

start nginx.exe --启动nginx

nginx.exe –s stop --停止nginx

nginx.exe –s reload–重启nginx

3、session共享

由于是负载均衡，所以Session需要共享，以下是使用Redis来存储Session达到共享目的。

1. 首先部署Redis

a) Redis安装包复制到服务器后，其中

redis-windows.conf是redis的配置文件，一般情况下只需要配置密码（可以不配置）

![Nginx负载均衡与Redis实现Tomcat的session共享基本配置][Nginx_Redis_Tomcat_session 2]

红框地方是密码。

b) 启动redis，打开控制台，进入到Redis根目录，

输入命令redis-server.exeredis.windows.conf

启动redis，成功界面如下：

![Nginx负载均衡与Redis实现Tomcat的session共享基本配置][Nginx_Redis_Tomcat_session 3]

2. tomcat配置，项目使用的是tomcat-redis-session-manager来把以前使用tomcat来存储session变成使用redis方式来存储，需要把

![Nginx负载均衡与Redis实现Tomcat的session共享基本配置][Nginx_Redis_Tomcat_session 4]

三个jar包复制到tomcat下的lib目录，然后配置tomcat管理session的方式:配置tomcat的context.xml文件，如下配置：

> 1.  <ValveclassNameValveclassName="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve"
> 2.  
> 3.  />
> 4.  
> 5.  <ManagerclassNameManagerclassName="com.orangefunction.tomcat.redissessions.RedisSessionManager"
> 6.  
> 7.  host="localhost"
> 8.  
> 9.  port="6379"
> 10. 
> 11. database="0"
> 12. 
> 13. maxInactiveInterval="60000"/>

host是redis服务器的ip，端口号是redis端口号，默认是6379.


[Nginx_Redis_Tomcat_session]: /pro/os/crawler/RIAY-E3EJ-JYAI.jpg
[Nginx_Redis_Tomcat_session 1]: /pro/os/crawler/AYEY-RFIB-JZMA.jpg
[Nginx_Redis_Tomcat_session 2]: /pro/os/crawler/F3M2-AYEA-EFQJ.jpg
[Nginx_Redis_Tomcat_session 3]: /pro/os/crawler/V2YN-YRVU-IMRA.jpg
[Nginx_Redis_Tomcat_session 4]: http://p9.pstatp.com/large/pgc-image/1524018213336371b6da272
 *  **原文作者：** 青峰科技
 *  **原文链接：** https://www.toutiao.com/item/6545610486862316039/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。