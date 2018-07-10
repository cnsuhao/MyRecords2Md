---
title: 基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）
date: 2018-05-11 17:29:01
categories: "开发"
tags:
	- 技术
	- 网易邮箱

---


# 1、引入pom.xml依赖 #

> <dependency>
> 
> <groupId>org.springframework.boot</groupId>
> 
> <artifactId>spring-boot-starter-mail</artifactId>
> 
> </dependency>

# 2、配置 application.properties #

> \# JavaMailSender 邮件发送的配置
> 
> spring.mail.host=smtp.163.com
> 
> spring.mail.username=用户163邮箱
> 
> spring.mail.password=邮箱密码
> 
> spring.mail.properties.mail.smtp.auth=true
> 
> spring.mail.properties.mail.smtp.starttls.enable=true
> 
> spring.mail.properties.mail.smtp.starttls.required=true

# 3、发送邮件通知  #

思路：邮件通知为单项的，邮件发送到用户端就可以。此邮件简单的标题和内容。


![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot]

发送邮件通知

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 1]

提示发送成功

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 2]

邮箱查看到测试邮件

# 4、发送邮件验证码  #

思路：发送验证码---邮箱接收--前端输入校验--校验成功与否，与邮件通知类似。


![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 3]

发送验证码

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 4]

发送成功

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 5]

邮箱查收

此处完成了验证码的发送，邮箱验证码的存储及校验，参考上一篇。

# 5、发送网页邮件  #

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 6]

发送网页邮件

# 6、发送附件 #

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 7]

发送附件邮件

# 7、静态资源 #

![基于SpingBoot发送邮件（通知、验证码、网页、附件、静态资源）][SpingBoot 8]

发送静态资源

发送邮件在我们的项目里还是很有必要的，以上只是小demo,可以根据自己的需求优化实现。



[SpingBoot]: /pro/os/crawler/QI7Z-ANBQ-UZIR.jpg
[SpingBoot 1]: /pro/os/crawler/FIFY-QUQI-FUYM.jpg
[SpingBoot 2]: /pro/os/crawler/VAAY-VEYE-VNAZ.jpg
[SpingBoot 3]: /pro/os/crawler/IZRF-FBN6-NUFA.jpg
[SpingBoot 4]: /pro/os/crawler/6VMQ-JYUF-VE6N.jpg
[SpingBoot 5]: /pro/os/crawler/NUIU-ZRU2-EJAA.jpg
[SpingBoot 6]: /pro/os/crawler/QEYZ-UMAN-7NVJ.jpg
[SpingBoot 7]: /pro/os/crawler/3EJF-IYZI-Y6ZJ.jpg
[SpingBoot 8]: /pro/os/crawler/EEFB-QBIB-NZQR.jpg
 *  **原文作者：** 好好学Java
 *  **原文链接：** https://www.toutiao.com/item/6554252985726140941/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。