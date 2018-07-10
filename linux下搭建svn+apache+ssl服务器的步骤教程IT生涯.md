---
title: linux下搭建svn+apache+ssl服务器的步骤教程
date: 2018-04-17 08:45:15
categories: "开发"
tags:
	- 文本编辑器
	- Linux
	- Vim
	- Apache
	- OpenSSL

---

（一）首先安装svn与ssl模块

yum install httpd mod\_dav\_svn subversion mod\_ssl

验证SVN是否安装成功：svn –version

查看httpd配置文件/etc/httpd/conf/httpd.conf。

如果有以上mod\_dav.so和mod\_dav\_fs.so两个文件的配置，则安装成功。

（二）配置版本库相关文件

1、创建主目录–创建版本库

mkdir -pv /svn/data

2、创建仓库

svnadmin create /svn/data/project

3、更改权限

因为是使用root权限创建的文件，需要赋予apache用户权限，否则apache没有足够权限去操作svn文件，所以要进行如下修改

chmod -R 700 /svn/data/ –修改库的其他人无权限

chown -R apache:apache /svn/data/ –修改库的所属

4、更改apache配置

vim /etc/httpd/httpd.conf/subversion.conf

加入以下内容

<location svn="">

DAV svn

SVNParentPath "/svn/data" \#改为刚创建的svn路径

AuthType Basic

AuthName "Subverion Repository"

AuthUserFile "/svn/passwd" \#改为密码文件路径

AuthzSVNAccessFile "/svn/authz" \#改为权限文件路径

Require valid-user \#需要验证用户

SSLRequireSSL \#默认使用ssl进行连接

</location>

5、创建apache账户

通过htpasswd命令创建用户

htpasswd -c /svn/passwd match

htpasswd -c /svn/passwd child

6、 设定SVN权限

vim /svn/auth.conf

加入以下代码：

\[groups\]

admin = match,chlid

\[/\]

match= rw

\[project:/\]

child= rw

match用户拥有/svn/data/根目录读写权限，而child拥有子目录project库读写权限。

(三) 使用SSL加密

1、生产密钥文件

cd /etc/httpd/conf

openssl genrsa -out httpd.key 1024

openssl req -new -key httpd.key -out httpd.pem -days 3650 -x509

按提示填写玩基本信息

完成后会生成3个证书相关文件

2、 修改apache使ssl生效

vim /etc/httpd/conf.d/ssl.conf

\#ssl的监听端口为443

Listen 443

\#此处为刚刚创建的crt地址

SSLCertificateFile /opt/key/server.crt

\#此处为刚刚创建的key地址

SSLCertificateKeyFile /opt/key/server.key

vim /etc/httpd/conf/httpd.conf

\#此处端口不使用

\#Listen 80

3、最后启动服务：

service httpd start

svnserve -d -r /svn/data/

通过https://服务器的ip地址/svn/project访问。提示下载证书，则证明SSL已经OK。

PS：如果出现405错误，则说明路径地址错误，输入正确的即可。
 *  **原文作者：** IT生涯
 *  **原文链接：** https://www.toutiao.com/item/6545211967605309965/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。