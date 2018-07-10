---
title: 使用jsencrypt实现参数的前端加密
date: 2017-03-30 19:34:51
categories: "开发"
tags:
	- 科技
	- Java
	- GitHub
	- OpenSSL

---

![使用jsencrypt实现参数的前端加密][jsencrypt]

在做项目中的登录功能时一般是通过form表单或者ajax方式将参数提交到服务器进行验证，在这个过程中，在前端对登录密码先进行一次加密的话，安全性肯定要优于直接提交的方式。最近在看博客园的登录页面时发现博客园的登录是用ajax发送http请求的方式，并在前端采用了加密，是采用jsencypt在前端进行加密的。后面查阅资料后了解到淘宝、京东也有用jsencypt库对登录密码进行前端加密的操作。jsencypt具体的使用参考官网，自行百度。

下面简单介绍基本的使用：

**创建密钥对JKS格式keystore：**

![使用jsencrypt实现参数的前端加密][jsencrypt 1]

**将JKS格式keystore转换成PKCS12证书文件：**

![使用jsencrypt实现参数的前端加密][jsencrypt 2]

**使用OpenSSL工具从PKCS12证书文件导出密钥对：**

![使用jsencrypt实现参数的前端加密][jsencrypt 3]

**从密钥对中提取出公钥：**

![使用jsencrypt实现参数的前端加密][jsencrypt 4]

拿到公钥test\_public.pem后，在cat test\_public.pem查看这个公钥内容，内容是base64格式的，这个公钥就是供在前端用jsencrypt对登录密码等参数进行RSA加密用的，看下test\_public.pem内容：（这里复制github上的过来，读者可以自行尝试）  


![使用jsencrypt实现参数的前端加密][jsencrypt 5]

接下来用一段简单的前端代码演示下jsencrypt的使用：  


![使用jsencrypt实现参数的前端加密][jsencrypt 6]

下面演示服务端解密过程，以Java为例。  


![使用jsencrypt实现参数的前端加密][jsencrypt 7]

当然在服务端除了上面这样的处理方式外，也可以根据实际业务场景使用不同的处理方式。  



[jsencrypt]: /pro/os/crawler/IBEQ-EQUY-EZVA.jpg
[jsencrypt 1]: /pro/os/crawler/JUBY-MJFE-7ZBB.jpg
[jsencrypt 2]: /pro/os/crawler/FFQU-U3NE-3IQQ.jpg
[jsencrypt 3]: /pro/os/crawler/UNRZ-NM2Y-MNY2.jpg
[jsencrypt 4]: /pro/os/crawler/BAIQ-EZMJ-AIIV.jpg
[jsencrypt 5]: /pro/os/crawler/VYYV-JAZQ-UFFI.jpg
[jsencrypt 6]: /pro/os/crawler/6ZEJ-2UBJ-3QMR.jpg
[jsencrypt 7]: /pro/os/crawler/ZIMJ-RURV-UZAR.jpg
 *  **原文作者：** weknow我是IT人
 *  **原文链接：** https://www.toutiao.com/item/6402881566434918913/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。