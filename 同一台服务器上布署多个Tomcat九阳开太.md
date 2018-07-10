---
title: 同一台服务器上布署多个Tomcat
date: 2018-04-22 23:06:21
categories: "开发"
tags:
	- 防火墙
	- Tomcat
	- Sed
	- 技术

---

1.  在各自的conf目录下的server.xml中修改各个tomcat的端口号，需要同是修改关闭端口和启用端口，确保各个端口都不同。

![同一台服务器上布署多个Tomcat][Tomcat]

关闭端口

![同一台服务器上布署多个Tomcat][Tomcat 1]

启动端口

2.配置tomcat的环境变量，名称可自定

TOMCAT\_HOME\_1=/usr/tomcat7\_8081

CATALINA\_HOME\_1=/usr/tomcat7\_8081

CATLINA\_BASE\_1=/usr/tomcat7\_8081

export TOMCAT\_HOME\_1 CATALINA\_HOME\_1 CATALINA\_BASH\_1

TOMCAT\_HOME\_2=/usr/tomcat7\_8082

CATALINA\_HOME\_2=/usr/tomcat7\_8082

CATLINA\_BASE\_2=/usr/tomcat7\_8082

export TOMCAT\_HOME\_2 CATALINA\_HOME\_2 CATALINA\_BASH\_2

3.修改各个bin下的catalian.sh中的CATALINA\_HOME的值（最好先备份一份），与环境变量中配的一致，可用如下命令

（1）sed 's/CATALINA\_HOME/CATALINA\_HOME\_1/g' /usr/tomcat7\_8081/bin/catalina.sh >catalina.sh\_1

（2）mv catalina.sh\_1 catalina.sh \#将新生成的文件重命名

（3）chmod a+x catalina.sh \#给新文件加执行权限

4.浏览器访问并解决防火墙问题。

在浏览器使用ip进行访问（端口默认：8080）可以看到tomcat的管理界面。192.168.0.14 为服务器的ip地址，如果访问不了，有可能是服务器防火墙问题，8080端口被拦截了，于是需要打开8080端口，并保存重启防火墙：
\[root@localhost bin\]\# iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
\[root@localhost bin\]\# /etc/init.d/iptables save
\[root@localhost bin\]\# /etc/init.d/iptables restart


[Tomcat]: http://p3.pstatp.com/large/pgc-image/152440831717971d1d8b45c
[Tomcat 1]: http://p1.pstatp.com/large/pgc-image/1524408355241985e0137f2
 *  **原文作者：** 九阳开太
 *  **原文链接：** https://www.toutiao.com/item/6547288913147331076/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。