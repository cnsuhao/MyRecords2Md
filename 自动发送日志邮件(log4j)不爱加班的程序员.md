---
title: 自动发送日志邮件(log4j)
date: 2017-10-31 12:33:56
categories: "开发"
tags:
	- 技术

---

Log4j具有记录系统日志的功能。通过配置也可将日志文件发送给特定的邮箱（系统管理员）

![自动发送日志邮件(log4j)][log4j]

1.  环境的准备jar

log4j-1.2.15.jar（版本低于log4j-1.2.14.jar不支持SMTP认证）

mail.jar

activation.jar

![自动发送日志邮件(log4j)][log4j 1]

2.邮箱帐号的设置

![自动发送日志邮件(log4j)][log4j 2]

3.系统日志的设置

![自动发送日志邮件(log4j)][log4j 3]

4.配置文件的修改

![自动发送日志邮件(log4j)][log4j 4]

5.邮件发送测试

![自动发送日志邮件(log4j)][log4j 5]

6.注意事项

![自动发送日志邮件(log4j)][log4j 6]


[log4j]: /pro/os/crawler/AZVU-UYVJ-JV3Q.jpg
[log4j 1]: /pro/os/crawler/IFVA-ZFIE-AB3Q.jpg
[log4j 2]: /pro/os/crawler/FZME-ABJV-FYFA.jpg
[log4j 3]: /pro/os/crawler/AYRV-IU7J-QVVF.jpg
[log4j 4]: /pro/os/crawler/ZN7Z-JAMB-UYIQ.jpg
[log4j 5]: /pro/os/crawler/IBAB-A3Q3-AEY3.jpg
[log4j 6]: /pro/os/crawler/RMEZ-VMVU-BUB2.jpg
 *  **原文作者：** 不爱加班的程序员
 *  **原文链接：** https://www.toutiao.com/item/6482928590580089358/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。