---
title: 简单搭建ngrok服务器，实现内网穿透
date: 2018-03-30 10:23:07
categories: "开发"
tags:
	- Git
	- Linux
	- Docker
	- Windows
	- OpenSSL

---

摘要: 快速搭建ngrok服务器，或者直接运行我制作的ngrok服务器的镜像来启动

为啥搭建ngrok服务器

在做Web开发时，我们需要将一个本地的Web网站暴露给外网访问（比如我在做本地开发微信时）。

ngrok 是一个反向代理工具，我们可以直接下载官网的客户端使用，但是因为官网服务器在国外，比较慢，而且免费版不支持绑定二级域名。

需要准备什么

 *  公网的linux服务器（最好是centos7，一步通过）
 *  独立域名

--------------------

如果对docker熟悉的话可以直接运行我制作的**ngrok服务器的镜像**来启动https://github.com/jueying/docker-ngrok-server

步骤

1. 安装git, golang和openssl

``````````
yum install -y git golang openssl
``````````

git版本和golang版本不能太旧，centos7默认安装git1.8.3，go1.8.3

2. clone ngrok项目到本地

``````````
git clone https://github.com/inconshreveable/ngrok.git /usr/local/ngrok
``````````

3. 生成证书

``````````
# 这里替换为自己的独立域名export NGROK_DOMAIN="huahongbin.cn"#进入到ngrok目录生成证书cd /usr/local/ngrok# 下面的命令用于生成证书openssl genrsa -out rootCA.key 2048openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pemopenssl genrsa -out device.key 2048openssl req -new -key device.key -subj "/CN=$NGROK_DOMAIN" -out device.csropenssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000# 将我们生成的证书替换ngrok默认的证书cp rootCA.pem assets/client/tls/ngrokroot.crtcp device.crt assets/server/tls/snakeoil.crtcp device.key assets/server/tls/snakeoil.key
``````````

4. 编译不同平台的服务端和客户端

``````````
# 编译64位linux平台服务端GOOS=linux GOARCH=amd64 make release-server# 编译64位windows客户端GOOS=windows GOARCH=amd64 make release-server# 如果是mac系统，GOOS=darwin。如果是32位，GOARCH=386
``````````

执行后会在ngrok/bin目录及其子目录下看到服务端`ngrokd`和客户端`ngrok.exe`。

5. 启动服务端

``````````
# 指定我们刚才设置的域名，指定http, https, tcp端口号，端口号不要跟其他程序冲突./bin/ngrokd -domain="$NGROK_DOMAIN" -httpAddr=":80" -httpsAddr=":8082" -tunnelAddr=":443"
``````````

6. 启动客户端

将ngrok.exe拷贝到本地文件夹中（可以用winscp），并在文件夹新建配置文件ngrok.cfg，内容如下：

``````````
server_addr: "huahongbin.cn:443"trust_host_root_certs: false
``````````

域名替换为自己的独立域名，端口替换为启动ngrok服务器设置的tunnel端口。

然后在cmd中使用以下命令启动：

``````````
ngrok.exe -subdomain=jueying -config=ngrok.cfg 80
``````````

80即为你要代理的本地端口

在浏览器中输http://127.0.0.1:4040 可以看到具体的请求信息。

常见问题

 *  编译时在下面步骤卡主`go get gopkg.in/yaml.v1`这是因为Git版本太低，请将服务器git版本升级到1.7.9.5以上。
 *  因为ngrok首次编译时需要在国外网站下载一些依赖。可能会很慢甚至timeout。多尝试几次，或者你懂得。
 *  **原文作者：** IT技术之家
 *  **原文链接：** https://www.toutiao.com/item/6538557657412796941/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。