---
title: python的web服务器：flask+boostrap 实现网站框架，源码分享
date: 2018-02-05 00:07:41
categories: "开发"
tags:
	- CSS
	- 编程语言
	- HTML
	- Flask
	- Python

---

先介绍一下flask中必须的两个功能。

# 模板渲染 #

用python生成html十分无趣，而且相当繁琐，因为你必须手动对html做转义来保证应用的安全。为此，flask配备了Jinja2模板引擎。

你可以使用render\_template()方法来渲染模板。你需要做的一切就是将模板名和你想作为关键字的参数传入模板的变量。举例如下：

首先在你的代码中添加from flask import render\_template

然后再添加如下函数

![python的web服务器：flask+boostrap 实现网站框架，源码分享][python_web_flask_boostrap]

flask会在templates文件夹中寻找模板，所以如果你的应用是个模块，那templates文件夹应该与模块同级；如果它是一个包，那这个templates文件夹作为包的子目录。

我们再看一下hello.html文件的内容

![python的web服务器：flask+boostrap 实现网站框架，源码分享][python_web_flask_boostrap 1]

然后运行你的app，并用浏览器访问/hello或者/hello/username的网址，应该都能获得相应的页面。这里就不多说了。

在模板里，你也可以访问request，session和g对象，以及get\_flashed\_messages()函数。

# 静态文件 #

动态web应用也会需要静态文件，通常是CSS和js文件。理想状��下，你已经配置好web服务器来提供静态文件，但是在开发中，flask也可以做到。只要在你的包中或者模块所在的目录中创建一个名为static的文件夹，在应用中使用/static即可访问。

给静态文件生成URL，使用特殊的‘static’端点名：

url\_for('static',filename='style.css')

这个文件应该存储在文件系统上的static/style.css

# bootstrap应用 #

好了，学到这里，我们就可以使用bootstrap前端来实现���个网站的基本框架了。

1.首先去bootstrap网站下载一个前端的例子

网站列出了许多例子，小编选择了最后一个，Carousel jumbotron。打开这个例子的链接 http://getbootstrap.com/2.3.2/examples/carousel.html ，小编要做的就是在自己的网站上实现同样的网页。用如下命令可以下载网页的全部内容。

sudo wget -r http://getbootstrap.com/2.3.2/examples/carousel.html

下载完成后进入路径getbootstrap.com/2.3.2，你应��能看到两个文件夹assets和examples. assets里面全是静态文件，examples里面只有一个carousel.html文件，我把这个文件改成index.html文件了。

2.首先创建一个简单的应用

其实小编也没有创建新的应用，就是使用了hello.py，在hello.py里面添加了如下代码：

![python的web服务器：flask+boostrap 实现网站框架，源码分享][python_web_flask_boostrap 2]

3.把bootstrap文件放入flask工程

这里只有两个步骤，把index.html文件放入templates文件夹中，把assets里面的内容放入static文件夹。templates和static文件夹如果不存在，请自行创建。

4.修改index.html文件

由于静态文件的路径改变了，因此需要在index.html中修改引用静态文件的路径。修改方法都是一致的，例如"../assets/css/bootstrap.css" 修改为"static/css/bootstrap.css".

最终显示效果如下：

![python的web服务器：flask+boostrap 实现网站框架，源码分享][python_web_flask_boostrap 3]

# 源代码分享 #

私信我，内容为"bootstrap",系统会自动回复链接哦。


[python_web_flask_boostrap]: /pro/os/crawler/ZFIB-MYBJ-VIR3.jpg
[python_web_flask_boostrap 1]: /pro/os/crawler/U36B-E2EY-M3QY.jpg
[python_web_flask_boostrap 2]: /pro/os/crawler/MNR3-YFV6-3UVR.jpg
[python_web_flask_boostrap 3]: /pro/os/crawler/7RNU-EEEN-3IB2.jpg
 *  **原文作者：** 小辉算个球
 *  **原文链接：** https://www.toutiao.com/item/6518731542217359885/
 *  **版��声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。