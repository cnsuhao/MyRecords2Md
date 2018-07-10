---
title: Github遇到Permanently added the RSA host key for IP address '192.30.252.128' to the list of known host
date: 2016-10-16 17:58:07
categories: "开发"
tags:
	- github
	- 解决方案
	- permanenty
	- gitbash
	- windows
	- 常见问题

---

刚开始使用github的时候不是很了解，新手一般的都会遇到这个问题Permanently added the RSA host key for IP address ‘192.30.252.128’ to the list of known hosts。其实这只是一个警告无伤大雅，继续用就是了，但是看着就是不爽，然后就想办法把他KO，一招致命。

出现的问题如下图：
![这里写图片描述][JBMB-YAEQ-2U3A.jpg]

上述那条警告的大概意思就是：警告：为IP地址192.30.252.128的主机（RSA连接的）持久添加到hosts文件中，那就来添加吧！

解决办法：

　　vim /etc/hosts

添加一行：192.30.252.128　　github.com

效果如图：

　　![这里写图片描述][7NNA-J3EM-VN7F.jpg]

这样就可以了，测试如图：
![这里写图片描述][BIMN-IQBY-BIZV.jpg]

此时那个警告就没有了。

如有转载请请务必保留此出处：http://www.cnblogs.com/xiangyangzhu/


[JBMB-YAEQ-2U3A.jpg]: /pro/os/crawler/JBMB-YAEQ-2U3A.jpg
[7NNA-J3EM-VN7F.jpg]: /pro/os/crawler/7NNA-J3EM-VN7F.jpg
[BIMN-IQBY-BIZV.jpg]: /pro/os/crawler/BIMN-IQBY-BIZV.jpg
 *  **原文作者：** zhoucheng05_13
 *  **原文链接：** https://blog.csdn.net/zhoucheng05_13/article/details/52831703
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。