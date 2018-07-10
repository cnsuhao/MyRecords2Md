---
title: 5分钟在laravel框架下完成网站搭建
date: 2018-01-29 07:30:30
categories: "开发"
tags:
	- Pages
	- Nginx
	- 编程语言
	- PHP
	- OpenSSL

---

> 编者按：最近给各位分享了Laravel框架，很多童鞋就喷：还是国内ThinkPHP框架好。当然这是仁者见仁，智者见智的了。上次小编发表了文章：[安装 Laravel，就这么几步搞定！][Laravel]反正小编觉得Laravel还真的不错，国外成熟的插件和模板也是不少的，今天小编就给大家分享一下如何5分钟在laravel框架下完成网站搭建。

做网站少不了CMS，也就是内容管理系统，小编就上网搜了下Laravel 网站方面的内容，就搜到了October，这是一个内容管理系统（CMS），更是一个致力于让开发工作流变得简单的web平台。

# 下载 #

起初小编还是想通过命令


> composer create-project october/october public dev-master

来下载安装，如下图，但是失败，小编忘了自己的openssl没开，网站证书也没有（：。

![5分钟在laravel框架下完成网站搭建][5_laravel]

最后还是通过直接下载解压也很容易解决。

![5分钟在laravel框架下完成网站搭建][5_laravel 1]

![5分钟在laravel框架下完成网站搭建][5_laravel 2]

解压到网站根目录

# 安装  #

跟其它CMS系统一样，运行http://你的域名/install.php，按下面这些图示一步一步安装就行了。


![5分钟在laravel框架下完成网站搭建][5_laravel 3]

系统检测

![5分钟在laravel框架下完成网站搭建][5_laravel 4]

同意并继续

![5分钟在laravel框架下完成网站搭建][5_laravel 5]

数据库相关--请先建一个空数据库

![5分钟在laravel框架下完成网站搭建][5_laravel 6]

管理员信息

![5分钟在laravel框架下完成网站搭建][5_laravel 7]

最后一步：如何创建你的网站？我选中间

![5分钟在laravel框架下完成网站搭建][5_laravel 8]

安装完成，给出前后台访问链接

# 设置  #

到了上一步，前台能访问，但CSS不起作用，网站布局不理想，后台不能访问，WHY?原来网址重写还没有配置。小编的本机环境是Nginx+php+mysql，所以小编按下图对Nginx.conf文件进行了进行了配置。


![5分钟在laravel框架下完成网站搭建][5_laravel 9]

主要是就下面这些粗体字内容：


> location / \{
> 
> index index.html index.htm index.php l.php;
> 
> autoindex off;
> 
>  **try\_files $uri /index.php$is\_args$args;**
> 
> \}
> 
> **rewrite ^themes/.\*/(layouts|pages|partials)/.\*.htm /index.php break;**
> 
> **rewrite ^bootstrap/.\* /index.php break;**
> 
> **rewrite ^config/.\* /index.php break;**
> 
> **rewrite ^vendor/.\* /index.php break;**
> 
> **rewrite ^storage/cms/.\* /index.php break;**
> 
> **rewrite ^storage/logs/.\* /index.php break;**
> 
> **rewrite ^storage/framework/.\* /index.php break;**
> 
> **rewrite ^storage/temp/protected/.\* /index.php break;**
> 
> **rewrite ^storage/app/uploads/protected/.\* /index.php break;**

然后网站就能正常访问了。


# 后台简单设置 #

都是英文，没有内容，怎么办？请进入后台http://你的域名/backend


![5分钟在laravel框架下完成网站搭建][5_laravel 10]

后台管理页面

October后台管理功能很方便，因为小编我刚才选择的是Vanilla主题，如下图

![5分钟在laravel框架下完成网站搭建][5_laravel 11]

这个主题包含了账号管理、博客、论坛三大组件，所有后台也就有下图几个主要方面可以进行管理设置。

![5分钟在laravel框架下完成网站搭建][5_laravel 12]

而针对CMS则主要有pages、Partials、Layouts、Content、Assets和components几个方面进行管理设置，超级方便呢。


小编我就主页default.htm进行了一点修改，加入了post.list插件，于是就有了下面的效果。不赖吧！

![5分钟在laravel框架下完成网站搭建][5_laravel 13]

修改后的前台主页

当然，对博客文章进行添加后，其Blog页面也就有了新的内容。

![5分钟在laravel框架下完成网站搭建][5_laravel 14]

添加博客文章后的Blog页面

小伙伴们，更多的插件和主题等待你去发现和应用，这样搭积木一样就搭建好了你的网站系统，你说好不好吧？


[Laravel]: http://m.toutiao.com/i6515322440363540999/?group_id=6515322440363540999&amp;group_flags=0
[5_laravel]: /pro/os/crawler/BQRN-NEI6-3MFE.jpg
[5_laravel 1]: /pro/os/crawler/3AAV-AZAI-ANYA.jpg
[5_laravel 2]: /pro/os/crawler/EU6J-FFBM-NBYF.jpg
[5_laravel 3]: /pro/os/crawler/Z73A-B3JN-2MMF.jpg
[5_laravel 4]: /pro/os/crawler/7JQB-FEBZ-NNVA.jpg
[5_laravel 5]: /pro/os/crawler/7BBZ-AN3U-NMUJ.jpg
[5_laravel 6]: /pro/os/crawler/Z2QM-VAQA-NAZM.jpg
[5_laravel 7]: /pro/os/crawler/3YUY-FFYN-VZUA.jpg
[5_laravel 8]: /pro/os/crawler/6JB2-QIER-FEMR.jpg
[5_laravel 9]: /pro/os/crawler/BYEU-7RNA-6VBB.jpg
[5_laravel 10]: /pro/os/crawler/6FIF-BNUZ-ARBU.jpg
[5_laravel 11]: /pro/os/crawler/FMNQ-M3JB-BNEA.jpg
[5_laravel 12]: /pro/os/crawler/JEJ6-ZIAV-AQVB.jpg
[5_laravel 13]: /pro/os/crawler/VNBY-IEUI-JIQF.jpg
[5_laravel 14]: /pro/os/crawler/NN6F-EIIU-EFNM.jpg
 *  **原文作者：** 小牛科技
 *  **原文链接：** https://www.toutiao.com/item/6516087376471654919/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。