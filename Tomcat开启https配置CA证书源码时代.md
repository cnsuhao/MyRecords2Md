---
title: Tomcat开启https配置CA证书
date: 2018-04-23 18:08:18
categories: "开发"
tags:
	- Tomcat
	- Scheme
	- 技术

---

1）登录到阿里云网站进入控制台

2）找到安全（云盾） 选择下面的CA证书服务如下图

![Tomcat开启https配置CA证书][Tomcat_https_CA]

3）找到上次申请证书的域名 点击旁边的 下载 如下图:

![Tomcat开启https配置CA证书][Tomcat_https_CA 1]

4)进入下载页面后选择相应的服务器---例如Tomcat 如下图

![Tomcat开启https配置CA证书][Tomcat_https_CA 2]

4)将下载后的文件解压 复制证书文件到Tomcat的cert(如果没有请创建)目录中

5)进入服务器中找到Tomcat进行配置 例如:找到安装Tomcat目录下该文件server.xml,一般默认路径都是在 conf 文件夹中。找到 <Connection port="8443"标签，增加如下属性：

<Connector port="8443"

protocol="HTTP/1.1"

SSLEnabled="true"

scheme="https"

secure="true"

keystoreFile="cert/证书.pfx"

keystoreType="PKCS12"

keystorePass="证书密码"

clientAuth="false"

SSLProtocol="TLSv1+TLSv1.1+TLSv1.2"

ciphers="TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA,TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA,TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA,TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256,TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256,TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256"/>

6)重启 Tomcat。

7)通过 https 方式访问您的站点，测试站点证书的安装配置


[Tomcat_https_CA]: http://p3.pstatp.com/large/pgc-image/1524478097105fcc09f3aa1
[Tomcat_https_CA 1]: http://p3.pstatp.com/large/pgc-image/1524478097274e64ed3b9d0
[Tomcat_https_CA 2]: http://p3.pstatp.com/large/pgc-image/1524478097402dbaab70eda
 *  **原文作者：** 源码时代
 *  **原文链接：** https://www.toutiao.com/item/6547583577410765325/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。