---
title: 从零开始搭建Nginx和Tomcat的web集群环境
date: 2018-01-04 00:11:54
categories: "开发"
tags:
	- Nginx
	- Java
	- Tomcat
	- 技术
	- HTML

---

一直以来对于web服务器对tomcat还是很熟悉了，但是很对于nginx还是有些陌生，一看到nginx的配置就让人有一种莫名的排斥，这就是对于陌生的恐惧，我们今天玩个有意思的，我从不了解nginx，到nginx和tomcat搭建集群，大概在不到一个小时内完成。

看看我这一个小时的学习成果，说不上对你有帮助。

首先nginx是出自俄罗斯的一款轻量级web服务器，开源免费，而且至简。

它的网站是这个：http://nginx.org/en/download.html

下载的版本目前有三类，比较容易理解，一个是目前的开发版本（Mainline version）,第二类是稳定的最新版，比如目前最新的是1.12.2的版本，有源码包和windows版本。第三类算是怀旧稳定版，不一定线上的环境都是最新的，也考虑了兼容性，算是比较贴心吧。

![从零开始搭建Nginx和Tomcat的web集群环境][Nginx_Tomcat_web]

说nginx至简，一个原因就是这个安装包确实够小，压缩版本不到1M,而解压后的版本也大概在7M左右。

```
[root@localhost nginx]# ll

total 960

-rw-r--r--. 1 root root 981687 Oct 18 13:14 nginx-1.12.2.tar.gz
```
安装nginx还是比较简便的，configure,make ,make install,需要注意的是安装是需要几个依赖包的，比如zlib,PCRE的库，可以提前检查下。

```
# rpm -qa|grep zlib

zlib-1.2.3-29.el6.x86_64

zlib-devel-1.2.3-29.el6.x86_64
```

PCRE的库需要的是pcre-devel，配置了yum源使用yum -y install pcre-devel即可搞定。

```
# rpm -qa|grep pcre

pcre-devel-7.8-7.el6.x86_64

pcre-7.8-7.el6.x86_64
```
小结下安装的三个步骤：

```
1.  ./configure --prefix=/usr/local/nginx
2.  make
3.  make install
```

如果需要编辑ssl额外加个选项。

nginx的启动确实很简单，直接使用nginx命令即可启动，默认是使用80端口，很快就能看到一个欢迎页面。

![从零开始搭建Nginx和Tomcat的web集群环境][Nginx_Tomcat_web 1]

当然我们可以通过fuser来检验80端口的情况，或者检测80端口是否被占用：

```
# fuser -n tcp 80

80/tcp: 21412 21413
```
或者是：

```
# netstat -pan | grep -w 80

tcp 0 0 0.0.0.0:80 0.0.0.0:\* LISTEN 21412/nginx

tcp 0 0 127.0.0.1:80 127.0.0.1:49593 TIME_WAIT -

tcp 0 0 192.168.253.219:57492 23.32.3.248:80 ESTABLISHED 21590/clock-applet

tcp 0 0 192.168.253.219:51678 60.221.218.180:80 TIME_WAIT -

tcp 1 0 ::ffff:192.168.253.21:39646 ::ffff:104.25.106.17:80 CLOSE_WAIT 12329/java

tcp 1 0 ::ffff:192.168.253.21:37445 ::ffff:104.25.107.17:80 CLOSE_WAIT 12329/java
```
如果查看nginx相关的进程，会发现有个master,有个worker的进程。

```
# ps -ef|grep nginx

root 21412 1 0 22:39 ? 00:00:00 nginx: master process ./nginx

nobody 21413 21412 0 22:39 ? 00:00:00 nginx: worker process

root 21719 15134 0 22:43 pts/3 00:00:00 grep nginx
```
这个部分怎么理解，可以通过nginx的配置文件就能容易理解了。在nginx.conf文件中，开头就是如下的两行。可以很明显看出worker进程有1个，配置了nobody，所以你看到的worker进程的属主就是nobody

```
# user nobody;

worker_processes 1;
```
这个是nginx的架构。他是使用epoll的方式。

nginx的命令几乎都不需要你重新去学习，直接使用-h就得到了帮助命令。所以我们很容易就会发现：./nginx -s stop 是停止的命令，启用配置文件使用-c选项。

在nginx所在的sbin目录下，一个完整的启动命令即为：

```
.nginx -c /usr/local/nginx/conf/nginx.conf
```
然后我们看看和tomcat怎么结合，nginx常用来做http服务器，反向代理，邮件服务器等。也是做负载均衡的一种很自然的方案。我们来简单模拟一下。

比如当前后端的服务器是tomcat，如果要实现负载均衡，通过nginx来转发就是一件很自然的事情，如果其中的一个tomcat出现问题，那也可以很方便的满足容错性。

为此我们需要配置若干个tomcat服务来模拟一下，比如我们使用3个tomcat。

```
drwxr-xr-x. 9 root root 4096 Jan 3 23:14 tomcat1

drwxr-xr-x. 9 root root 4096 Jan 3 23:14 tomcat2

drwxr-xr-x. 9 root root 4096 Jan 3 23:14 tomcat3
```

默认端口为8080，我们简单包装，三个tomcat的端口即为：

18080

28080

38080

修改tomcat的配置文件server.xml就需要注意以下几个地方的端口设置，分别为：

**tomcat1**:

```
<Server port="18005" shutdown="SHUTDOWN">

<Connector port="18080" protocol="HTTP/1.1"

<Connector port="18009" protocol="AJP/1.3" redirectPort="8443" />
```
**tomcat2**:

```
<Server port="28005" shutdown="SHUTDOWN">

<Connector port="28080" protocol="HTTP/1.1"

<Connector port="28009" protocol="AJP/1.3" redirectPort="8443" />
```
**tomcat3:**

```
<Server port="38005" shutdown="SHUTDOWN">

<Connector port="38080" protocol="HTTP/1.1"

<Connector port="38009" protocol="AJP/1.3" redirectPort="8443" />
```
然后启动做简单的验证：能看到小猫即可。

![从零开始搭建Nginx和Tomcat的web集群环境][Nginx_Tomcat_web 2]

为了区别起见，我们可以在webapps/ROOT/index.jsp里面分别表示tomcat1,tomcat2,tomcat3这样后面做转发就知道是到达了哪个tomcat了。

此时的tomcat是可以了，我们配置Nginx.

nginx的配置核心就是nginx.conf了。

注意红色的部分配置：

```
#gzip on;

upstream jeanron100.com {

server 127.0.0.1:18080 weight=1;

server 127.0.0.1:28080 weight=2;

server 127.0.0.1:38080 weight=3;

}

server {

listen 80;

server_name localhost;

#charset koi8-r;

#access_log logs/host.access.log main;

#location / {

# root html;

# index index.html index.htm;

#}

location / {

proxy_pass http://jeanron100.com;

proxy_redirect default;

}
```
然后启动nginx，使用命令：

```
./nginx -c /usr/local/nginx/conf/nginx.conf
```

然后在浏览器中输入IP和页面的名字。可以看到这个时候已经开始做了转发，现在调到了tomcat2上。

继续刷新，现在跳到了tomcat3上面。

![从零开始搭建Nginx和Tomcat的web集群环境][Nginx_Tomcat_web 3]

不断的刷新，tomcat和nginx是映射起来了。


[Nginx_Tomcat_web]: /pro/os/crawler/3AVU-J2BI-J7NR.jpg
[Nginx_Tomcat_web 1]: /pro/os/crawler/BE6N-EVNN-FJMU.jpg
[Nginx_Tomcat_web 2]: /pro/os/crawler/VIRR-YMZY-3QYF.jpg
[Nginx_Tomcat_web 3]: /pro/os/crawler/NQQM-E2UU-AJIZ.jpg
 *  **原文作者：** 杨建荣的学习笔记
 *  **原文链接：** https://www.toutiao.com/item/6506857905264787971/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。