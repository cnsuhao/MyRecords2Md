---
title: Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！
date: 2018-06-23 15:21:44
categories: "开发"
tags:
	- 网络爬虫
	- CSS
	- 技术
	- HTML

---

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup]

博主使用的是Mac系统，直接通过命令安装库：

``````````
sudo easy_install beautifulsoup4
``````````

安装完成后，尝试包含库运行：

``````````
from bs4 import BeautifulSoup
``````````

若没有报错，则说明库已正常安装完成。

**开始**

本文会通过这个网页http://reeoo.com来进行示例讲解，如下图所示

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 1]

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 2]

也可以通过文件句柄来初始化，可先将HTML的源码保存到本地同级目录 reo.html，然后将文件名作为参数：

``````````
soup = BeautifulSoup(open('reo.html'))
``````````

可以打印 soup，输出内容和HTML文本无二致，此时它为一个复杂的树形结构，每个节点都是Python对象。

Ps. 接下来示例代码中所用到的 soup 都为该soup。

**Tag**

Tag对象与HTML原生文档中的标签相同，可以直接通过对应名字获取

``````````
tag = soup.titleprint tag
``````````

打印结果：

``````````
<title>Reeoo - web design inspiration and website gallery</title>
``````````

**Name**

通过Tag对象的name属性，可以获取到标签的名称

``````````
print tag.name# title
``````````

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 3]

**tag中的字符串**

通过 string 方法获取标签中包含的字符串

``````````
tag = soup.titles = tag.stringprint s# Reeoo - web design inspiration and website gallery
``````````

**文档树的遍历**

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 4]

如下图：

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 5]

我们希望获取到 article 标签中的 li

``````````
tag = soup.article.div.ul.liprint tag
``````````

打印结果：

``````````
<li id="sponsor"><div class="sponsor_tips"></div><script async="" id="_carbonads_js" src="//cdn.carbonads.com/carbon.js?zoneid=1696&serve=CVYD42T&placement=reeoocom" type="text/javascript"></script></li>
``````````

也可以把中间的一些节点省略，结果也一致

``````````
tag = soup.article.li
``````````

通过 . 属性只能获取到第一个tag，若想获取到所有的 li 标签，可以通过 find\_all() 方法

``````````
ls = soup.article.div.ul.find_all('li')
``````````

获取到的是包含所有li标签的列表。

tag的 .contents 属性可以将tag的子节点以列表的方式输出:

``````````
tag = soup.article.div.ulcontents = tag.contents
``````````

打印 contents 可以看到列表中不仅包含了 li 标签内容，还包括了换行符 ' '

过tag的 .children 生成器,可以对tag的子节点进行循环

``````````
tag = soup.article.div.ulchildren = tag.childrenprint childrenfor child in children: print child
``````````

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 6]

**文档树的搜索**

对树形结构的文档进行特定的搜索是爬虫抓取过程中最常用的操作。

**find\_all()**

find\_all(name , attrs , recursive , string , \*\* kwargs)

**name 参数**

查找所有名字为 name 的tag

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 7]

指定名字的属性参数值可以包括：字符串、正则表达式、列表、True/False。

**True/False**

是否存在指定的属性。

搜索所有带有 target 属性的标签

``````````
soup.find_all(target=True)
``````````

搜索所有不带 target 属性的标签（仔细观察会发现，搜索结果还是会有带 target 的标签，那是不带 target 标签的子标签，这里需要注意一下。）

``````````
soup.find_all(target=False)
``````````

可以指定多个参数作为过滤条件，例如页面缩略图部分的标签如下所示：

``````````
...<li> <div class="thumb"> <a href="http://reeoo.com/aim-creative-studios">![AIM Creative Studios](http://upload-images.jianshu.io/upload_images/1346917-f6281ffe1a8f0b18.gif?imageMogr2/auto-orient/strip)</a> </div> <div class="title"> <a href="http://reeoo.com/aim-creative-studios">AIM Creative Studios</a> </div></li>...
``````````

搜索 src 属性中包含 reeoo 字符串，并且 class 为 lazy 的标签：

``````````
soup.find_all(src=re.compile("reeoo.com"), class_='lazy')
``````````

搜索结果即为所有的缩略图 img 标签。

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 8]

打印搜索结果可看到包含3个元素，分别是对应标签里的内容，具体见下图所示

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 9]

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 10]

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 11]

**limit 参数**

find\_all() 返回的是整个文档的搜索结果，如果文档内容较多则搜索过程耗时过长，加上 limit 限制，当结果到达 limit 值时停止搜索并返回结果。

搜索 class 为 thumb 的 div 标签，只搜索3个

``````````
soup.find_all('div', class_='thumb', limit=3)
``````````

打印结果为一个包含3个元素的列表，实际满足结果的标签在文档里不止3个。

**recursive 参数**

find\_all() 会检索当前tag的所有子孙节点,如果只想搜索tag的直接子节点,可以使用参数 recursive=False。

![Beautiful Soup是一个爬虫的神级库！今天教你完全摸透它！][Beautiful Soup 12]

**CSS选择器**

Tag 或 BeautifulSoup 对象通过 select() 方法中传入字符串参数, 即可使用CSS选择器的语法找到tag。

语义和CSS一致，搜索 article 标签下的 ul 标签中的 li 标签

``````````
print soup.select('article ul li')
``````````

通过类名查找，两行代码的结果一致，搜索 class 为 thumb 的标签

``````````
soup.select('.thumb')soup.select('[class~=thumb]')
``````````

通过id查找，搜索 id 为 sponsor 的标签

``````````
soup.select('#sponsor')
``````````

通过是否存在某个属性来查找，搜索具有 id 属性的 li 标签

``````````
soup.select('li[id]')
``````````

通过属性的值来查找查找，搜索 id 为 sponsor 的 li 标签

``````````
soup.select('li[id="sponsor"]')
``````````

**其他**

其他的搜索方法还有：

find\_parents() 和 find\_parent()

find\_next\_siblings() 和 find\_next\_sibling()

find\_previous\_siblings() 和 find\_previous\_sibling()

…

参数的作用和 find\_all()、find() 差别不大，这里就不再列举使用方式了。这两个方法基本已经能满足绝大部分的查询需求。

还有一些方法涉及文档树的修改。对于爬虫来说大部分工作只是检索页面的信息，很少需要对页面源码做改动，所以这部分的内容也不再列举。

**私信小编007 即可获取数十套PDF哦！**


[Beautiful Soup]: /pro/os/crawler/BYRB-IMEV-EIAJ.jpg
[Beautiful Soup 1]: /pro/os/crawler/N73Q-6VAQ-FJRE.jpg
[Beautiful Soup 2]: /pro/os/crawler/77VR-NUEQ-NVQR.jpg
[Beautiful Soup 3]: /pro/os/crawler/QIFV-ZNVZ-BBV3.jpg
[Beautiful Soup 4]: /pro/os/crawler/AIJA-FB3A-NRUR.jpg
[Beautiful Soup 5]: /pro/os/crawler/M6B2-I3EA-RYA3.jpg
[Beautiful Soup 6]: /pro/os/crawler/YRFY-E3EY-36N3.jpg
[Beautiful Soup 7]: /pro/os/crawler/YN2Q-7VM7-REEA.jpg
[Beautiful Soup 8]: /pro/os/crawler/IFRQ-JBZU-EARR.jpg
[Beautiful Soup 9]: /pro/os/crawler/UUMR-NZ2Y-FURJ.jpg
[Beautiful Soup 10]: /pro/os/crawler/BEYZ-YYU3-YMVB.jpg
[Beautiful Soup 11]: /pro/os/crawler/JFE2-AAYN-Q2UJ.jpg
[Beautiful Soup 12]: /pro/os/crawler/MNIM-YNNV-FYRE.jpg
 *  **原文作者：** 繁华落尽and曲终人散
 *  **原文链接：** https://www.toutiao.com/item/6570176848867623438/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。