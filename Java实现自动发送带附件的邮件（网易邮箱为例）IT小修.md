---
title: Java实现自动发送带附件的邮件（网易邮箱为例）
date: 2017-05-14 07:00:17
categories: "开发"
tags:
	- 科技
	- Java
	- 网易邮箱
	- 编程语言

---

![Java实现自动发送带附件的邮件（网易邮箱为例）][Java]

JAVA自动发送带附件的邮件

在手机流行之前，我们注册账号都会需要邮箱验证，及时是现在手机这么普及，在很多的时候也还是会用到邮箱来接收验证码或者验证token。今天就以网易邮箱作为发件邮箱服务器来示例一下通过JAVA自动发送携带附件的邮件。  


# 1、准备工作（jar包） #

mail.jar（Java邮箱操作核心jar包）

sun.misc.BASE64Decoder.jar（用来处理文件编程一遍上传附件的）

# 2、一个网易邮箱，并且设置POP3/SMTP/IMAP开启状态，如下图 #

![Java实现自动发送带附件的邮件（网易邮箱为例）][Java 1]

网易邮箱设置POP3/SMTP/IMAP

# 3、代码示例    #

![Java实现自动发送带附件的邮件（网易邮箱为例）][Java 2]

main方法  


![Java实现自动发送带附件的邮件（网易邮箱为例）][Java 3]

邮件内容实现方法  


上述代码亲测可以行，如果是其他邮箱服务器，只要改一下邮箱相应的host即可。这里说明一下，QQ邮箱应该是需要获取一个token秘钥，不是通过密码的方式来验证的。


[Java]: /pro/os/crawler/AIIB-Z3N7-JEEN.jpg
[Java 1]: /pro/os/crawler/EBA3-AJMB-FFMM.jpg
[Java 2]: /pro/os/crawler/BJEI-VM2I-NJMV.jpg
[Java 3]: /pro/os/crawler/6RIY-JEJB-RBMM.jpg
 *  **原文作者：** IT小修
 *  **原文链接：** https://www.toutiao.com/item/6419643509120172545/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。