---
title: 腾讯云香港服务器搭建SS-实验
date: 2018-04-23 12:09:25
categories: "开发"
tags:
	- 技术
	- Wget
	- 腾讯云计算

---

登录服务器：

看图片，文章不允许链接

![腾讯云香港服务器搭建SS-实验][SS-]

CVM主页

看见如下界面

![腾讯云香港服务器搭建SS-实验][SS- 1]

VNC登录

输入刚才设置的密码

腾讯云默认有wegt

如果没有，输入如下命令安装

![腾讯云香港服务器搭建SS-实验][SS- 2]

安装wget命令支持

wget –no-check-certificate （https://） raw.githubusercontent.com/teddysun/shadowsocks\_install/master/shadowsocks-libev.sh

文章限制链接，请操作者 自行去掉括号

![腾讯云香港服务器搭建SS-实验][SS- 3]

安装

chmod +x shadowsocks-libev.sh

./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log

![腾讯云香港服务器搭建SS-实验][SS- 4]

密码

输入ss设置的密码 这里为12345678

端口号 6666

![腾讯云香港服务器搭建SS-实验][SS- 5]

端口号设置

选择加密方式

![腾讯云香港服务器搭建SS-实验][SS- 6]

加密方式选择

接着任意键等待结束

![腾讯云香港服务器搭建SS-实验][SS- 7]

基本信息

部署完成 reboot一下 确保服务生效

配置客户端

![腾讯云香港服务器搭建SS-实验][SS- 8]

服务器配置

![腾讯云香港服务器搭建SS-实验][SS- 9]

代理设置

![腾讯云香港服务器搭建SS-实验][SS- 10]

效果图

![腾讯云香港服务器搭建SS-实验][SS- 11]

流量统计

请勿用于商业用途


[SS-]: http://p1.pstatp.com/large/pgc-image/1524456237018c5472036f8
[SS- 1]: http://p3.pstatp.com/large/pgc-image/152445626028530ef0f89b6
[SS- 2]: http://p3.pstatp.com/large/pgc-image/152445628243765d87fa51c
[SS- 3]: http://p9.pstatp.com/large/pgc-image/15244563164824030509a0e
[SS- 4]: http://p9.pstatp.com/large/pgc-image/1524456342588bd64ed31c5
[SS- 5]: http://p1.pstatp.com/large/pgc-image/1524456361206a836fe17ca
[SS- 6]: http://p1.pstatp.com/large/pgc-image/1524456378938a456aa0e34
[SS- 7]: http://p1.pstatp.com/large/pgc-image/152445639838459a813f667
[SS- 8]: http://p3.pstatp.com/large/pgc-image/1524456412180824e4fc475
[SS- 9]: http://p3.pstatp.com/large/pgc-image/1524456428653c03d5028c4
[SS- 10]: http://p9.pstatp.com/large/pgc-image/1524456445259264a60c208
[SS- 11]: http://p1.pstatp.com/large/pgc-image/15244564554942f59b5ea86
 *  **原文作者：** 美奔科技腾讯云
 *  **原文链接：** https://www.toutiao.com/item/6547491093158560259/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。