---
title: 利用nginx做反向代理解决前端的跨域问题
date: 2018-06-23 20:49:50
categories: "开发"
tags:
	- Nginx
	- 技术
	- Windows

---

首先我们要从nginx官网去下载nginx的压缩包，解压之后就可以用了，然后找到nginx目录下的nginx.conf文件，然后进行配置

配置如下：

![利用nginx做反向代理解决前端的跨域问题][nginx]

![利用nginx做反向代理解决前端的跨域问题][nginx 1]

到这一步我们的nginx就算是配置完成了，然后我们再说一下nginx常用的命令

启动：start nginx

停止： nginx -s quit

重新加载配置文件： nginx -s reload

查看windows任务管理器下Nginx的进程命令：tasklist /fi "imagename eq nginx.exe"

接下来我们启动nginx命令

然后看一下我们前端代码如何写

![利用nginx做反向代理解决前端的跨域问题][nginx 2]

到这里我们的nginx就算代理成功，另外在插一句，其实vue的config中的index.js配置的跨域，实现跨域的原理也和nginx

做反向代理的原理是一样的。。。。


[nginx]: /pro/os/crawler/NRJQ-MVUR-636J.jpg
[nginx 1]: /pro/os/crawler/JJVM-JV6F-IRQR.jpg
[nginx 2]: /pro/os/crawler/ZAZQ-IMVQ-NI32.jpg
 *  **原文作者：** Java高级架构技术
 *  **原文链接：** https://www.toutiao.com/item/6570261398507487757/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。