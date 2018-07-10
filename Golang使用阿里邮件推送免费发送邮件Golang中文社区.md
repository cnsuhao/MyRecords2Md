---
title: Golang使用阿里邮件推送免费发送邮件
date: 2018-05-20 15:36:46
categories: "开发"
tags:
	- 镜音双子
	- 技术
	- DNS
	- HTML

---

![Golang使用阿里邮件推送免费发送邮件][Golang]

大致分为两步:

一、登录邮件推送控制台，绑定并验证域名，然后查询您的域名配置信息。

二、登录您的域名注册商网站，将您查询的域名配置记录值逐条添加域名解析。

# 绑定域名及查询域名配置信息 #

先登录阿里云控制台，点击**邮件推送**

![Golang使用阿里邮件推送免费发送邮件][Golang 1]

点击**发信域名**，再点击**新建域名**

![Golang使用阿里邮件推送免费发送邮件][Golang 2]

再弹出框中填写域名，点击确定, 这里以domain.com域名作为示例，请自行修改为自己的真实域名。

![Golang使用阿里邮件推送免费发送邮件][Golang 3]

然后点击**配置**

![Golang使用阿里邮件推送免费发送邮件][Golang 4]

进入域名配置页面

![Golang使用阿里邮件推送免费发送邮件][Golang 5]

# 设置域名解析 #

按域名配置页面中的相关配置，增加域名解析。一共要增加两个TXT记录、一个MX记录、一个CNAME记录。如下图所示:

![Golang使用阿里邮件推送免费发送邮件][Golang 6]

# 验证域名配置 #

回到发信域名页面，点击验证

![Golang使用阿里邮件推送免费发送邮件][Golang 7]

# 新建发信地址 #

点击**发信地址**，再点击**新建发信地址**

![Golang使用阿里邮件推送免费发送邮件][Golang 8]

然后，**发信域名**选择刚刚验证通过的域名，如domain.com。**账号**随便填，如support。**回信地址**不填。 **发信类型**选触发邮件

![Golang使用阿里邮件推送免费发送邮件][Golang 9]

设置SMTP密码

![Golang使用阿里邮件推送免费发送邮件][Golang 10]

![Golang使用阿里邮件推送免费发送邮件][Golang 11]

# Golang发送邮件 #

> package main
> 
> import (
> 
> "crypto/tls"
> 
> "fmt"
> 
> "log"
> 
> "net"
> 
> "net/smtp"
> 
> )
> 
> // SendMail 发送邮件
> 
> func SendMail(host string, port int, email, password, emailFrom, toEmail, subject, content string) error \{
> 
> headers := make(map\[string\]string)
> 
> headers\["From"\] = emailFrom + "<" + email + ">"
> 
> headers\["To"\] = toEmail
> 
> headers\["Subject"\] = subject
> 
> headers\["Content-Type"\] = "text/html; charset=UTF-8"
> 
> message := ""
> 
> for key, value := range headers \{
> 
> message += fmt.Sprintf("%s: %s ", key, value)
> 
> \}
> 
> message += " " + content
> 
> auth := smtp.PlainAuth("", email, password, host)
> 
> err := sendMailUsingTLS(
> 
> fmt.Sprintf("%s:%d", host, port),
> 
> auth,
> 
> email,
> 
> \[\]string\{toEmail\},
> 
> message,
> 
> )
> 
> return err
> 
> \}
> 
> //参考net/smtp的func SendMail()
> 
> //使用net.Dial连接tls(ssl)端口时, smtp.NewClient()会卡住且不提示err
> 
> //len(to) > 1 时, to\[1\]开始提示是密送
> 
> func sendMailUsingTLS(addr string, auth smtp.Auth, from string,
> 
> to \[\]string, message string) error \{
> 
> client, err := createSMTPClient(addr)
> 
> if err != nil \{
> 
> fmt.Println(err.Error())
> 
> return err
> 
> \}
> 
> defer client.Close()
> 
> if auth != nil \{
> 
> if ok, \_ := client.Extension("AUTH"); ok \{
> 
> if err := client.Auth(auth); err != nil \{
> 
> log.Println(err.Error())
> 
> return err
> 
> \}
> 
> \}
> 
> \}
> 
> if err := client.Mail(from); err != nil \{
> 
> return err
> 
> \}
> 
> for \_, addr := range to \{
> 
> if err := client.Rcpt(addr); err != nil \{
> 
> return err
> 
> \}
> 
> \}
> 
> writeCloser, err := client.Data()
> 
> if err != nil \{
> 
> return err
> 
> \}
> 
> \_, err = writeCloser.Write(\[\]byte(message))
> 
> if err != nil \{
> 
> return err
> 
> \}
> 
> err = writeCloser.Close()
> 
> if err != nil \{
> 
> return err
> 
> \}
> 
> return client.Quit()
> 
> \}
> 
> func createSMTPClient(addr string) (\*smtp.Client, error) \{
> 
> conn, err := tls.Dial("tcp", addr, nil)
> 
> if err != nil \{
> 
> fmt.Println(err.Error())
> 
> return nil, err
> 
> \}
> 
> host, \_, \_ := net.SplitHostPort(addr)
> 
> return smtp.NewClient(conn, host)
> 
> \}
> 
> func main() \{
> 
> host := "smtpdm.aliyun.com" // smtp邮箱域名
> 
> port := 465
> 
> email := "support@domain.com" // 域名邮箱账号，即刚刚设置的发信地址
> 
> password := "xxxxxxxx" // SMTP密码
> 
> emailFrom := "domain" // 邮件来源
> 
> toEmail := "abc@163.com" // 收件人
> 
> subject := "你好" // 邮件标题
> 
> content := "邮件内容" // 邮件正文
> 
> SendMail(host, port, email, password, emailFrom, toEmail, subject, content)
> 
> \}

本文由**shen100**原创，**Golang中文社区**首发，现同步到头条。欢迎转载, 请在正文中标明原文链接及作者，谢谢。也欢迎来**Golang中文社区**学习、交流。


[Golang]: /pro/os/crawler/RZJY-VZ6B-YRRZ.jpg
[Golang 1]: /pro/os/crawler/2EAN-MENY-AZYY.jpg
[Golang 2]: /pro/os/crawler/YIFM-YERM-IABF.jpg
[Golang 3]: /pro/os/crawler/RMIV-IRBZ-N2Y3.jpg
[Golang 4]: /pro/os/crawler/JMJJ-IFMN-VMM2.jpg
[Golang 5]: /pro/os/crawler/QF6F-JIQQ-7BZF.jpg
[Golang 6]: /pro/os/crawler/MINI-MNFF-UJUF.jpg
[Golang 7]: /pro/os/crawler/IENU-2IBZ-EQ3M.jpg
[Golang 8]: /pro/os/crawler/JBZU-3UY2-QZFE.jpg
[Golang 9]: /pro/os/crawler/B2EV-BARI-IRIB.jpg
[Golang 10]: /pro/os/crawler/ME22-M2MA-NFZA.jpg
[Golang 11]: /pro/os/crawler/QRFV-2IRM-2ARJ.jpg
 *  **原文作者：** Golang中文社区
 *  **原文链接：** https://www.toutiao.com/item/6557562760006205959/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。