---
title: 教你如何用github搭建个人博客网站
date: 2017-11-09 18:53:35
categories: "开发"
tags:
	- Pages
	- Origin
	- 技术
	- GitHub
	- Windows

---

你想建立一个个人网站吗？但苦于昂贵的域名和空间，但不要慌，其实用github也可以搭建一个个人网站，使用github pages服务搭建博客的好处有有很多，比如免费快捷，不需要昂贵的域名和空间，也不需要后台，全是静态文件，我们都知道静态文件访问速度很快，如果你要真正建一个独立博客网站的话，在github上也可以打包下载，搬家到别的空间上，最主要的是可以绑定自己的域名，这样可以省很多的金钱。

一、Github Pages的注册与仓库的建立

1、首先你肯定得有一个Github账号，没有Github账号就别说了，我们可以去Github官网（github.com）注册一个。

![教你如何用github搭建个人博客网站][github]

2、有了账号以后,需要登录到Github，然后点击New repository新建仓库。

![教你如何用github搭建个人博客网站][github 1]

3、新建一个以你的用户名为前缀的.github.io的仓库，比如说，如果你的github用户名是123，那么你就新建的仓库就是123.github.io，也就是说你的网站访问地址就是 https://123.github.io（如图所示），然后选择，然后点击create repository。

![教你如何用github搭建个人博客网站][github 2]

4、这时候你的仓库就建立好了。

![教你如何用github搭建个人博客网站][github 3]

二、把源码上传到github仓库

1、首先，先下载git：https://git-scm.com/download/win，然后选择一个目录输入下面的指令：

git clone https://github.com/zekel123/zekel123-.github.io.git

![教你如何用github搭建个人博客网站][github 4]

2、然后我们会在你选择的目录看见生成了一个zekel123-.github.io文件夹。

![教你如何用github搭建个人博客网站][github 5]

3、进入文件下，把你的网站源码放进去，然后打开命令行进入到zekel123-.github.io.git目录下依次执行下面的命令：

git add .

git commit -m “上传网站源码到github”

git remote add origin https://github.com/zekel123/zekel123-.github.io.git

git push -u origin master

![教你如何用github搭建个人博客网站][github 6]

然后就大功告成了,下面是我建立的导航页（点击图片放大浏览）。

![教你如何用github搭建个人博客网站][github 7]


[github]: /pro/os/crawler/IZYR-ZFRR-NQEI.jpg
[github 1]: /pro/os/crawler/YEYQ-FJUR-ZYUU.jpg
[github 2]: /pro/os/crawler/MUJI-IQBR-VFJB.jpg
[github 3]: /pro/os/crawler/QEIV-YUUY-Z6ZF.jpg
[github 4]: /pro/os/crawler/6V2A-INVQ-6RAQ.jpg
[github 5]: /pro/os/crawler/73UM-MU73-6BIB.jpg
[github 6]: /pro/os/crawler/QEAY-VUUN-YNMU.jpg
[github 7]: /pro/os/crawler/ZEZQ-AQ3E-FIYU.jpg
 *  **原文作者：** 小小柚果
 *  **原文链接：** https://www.toutiao.com/item/6486366188921160205/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。