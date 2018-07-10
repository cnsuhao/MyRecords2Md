---
title: 骚年，还在为歌荒发愁吗?python教你爬取网易云热门歌单
date: 2018-06-15 07:21:00
categories: "开发"
tags:
	- JavaScript
	- 编程语言
	- Chrome
	- 网易
	- Python

---

**需求分析**

每当歌荒时,总想找那些播放量比较高的歌单听,毕竟这么多人选择的歌单歌曲质量总会有保证.

**爬取目标**

本文将提取网易云音乐 播放量在1000万 以上的歌单名称，播放量和链接地址.

**准备工作**

保证电脑安装了 python3.6 和已经安装好了 selenium 库.没安装的可通过 在终端输入 pip install selenium 进行安装.

**爬取分析**

打开歌单的 url:http://music.163.com/\#/discover/playlist.用 Chrome 的开发者工具 (F12) 分析.

![骚年，还在为歌荒发愁吗?python教你爬取网易云热门歌单][python]

页面详情

发现歌单信息在<ul class='m-cvrlst f-cb' id='m-pl-container'>下的 <li>标签中.

``````````
因此我们可以通过以下代码获取歌单信息:from selenium import webdriveurl = http://music.163.com/#/discover/playlistbrowser = webdrive.Chrome()brows.get(url)data = browser.find_element_by_id('m-pl-container').find_elements_by_tag_name('li')
``````````

但是我们会发现无法获取歌单信息,原因是注意看图片最前面发现有个 <iframe>标签,因此导致我们无法定位,故需要在获取数据前加一行代码定位browser.switch\_to.frame('contentFrame'),这样我们就可以获取数据了.关于selenium定位的问题可以访问https://blog.csdn.net/huilan\_same/article/details/52200586 更详细的了解.

**获取歌单详细信息**

![骚年，还在为歌荒发愁吗?python教你爬取网易云热门歌单][python 1]

页面详情

通过上图我们发现歌单名和地址在<a title="| 粤语 | 宇宙粒子与每个人的爱情" href="/playlist?id=2220301639" class="msk"></a>中,播放量在<span class="nb">14万</span>中.接下来我们来获取首页的歌单信息.**抓取首页**

``````````
from selenium import webdriverimport csvbrowser = webdr.Chrome()csv_file = open('playlist.csv', 'w', newline='', encoding='utf-8') #注意此处必须得加 encoding='utf-8,不然遇到歌单名包含特殊字符会报错writer = csv.writer(csv_file)writer.writerow(['标题', '播放数', '链接'])url = 'http://music.163.com/#/discover/playlist/?order=hot&cat=%E5%85%A8%E9%83%A8&limit=35&offset=0'browser.get(url)browser.switch_to.frame('contentFrame')data = browser.find_element_by_id('m-pl-container').find_elements_by_tag_name('li')for i in range(len(data)): nb = data[i].find_element_by_class_name('nb').text if '万' in nb and int(nb.split('万')[0]) > 1000: msk = data[i].find_element_by_css_selector('a.msk') writer.writerow([msk.get_attribute('title'), nb, msk.get_attribute('href')])csv_file.close()
``````````

**获取所有页面**

![骚年，还在为歌荒发愁吗?python教你爬取网易云热门歌单][python 2]

页面数据

通过分析我们发现每一页的地址只是 offset 发生了偏移,即每一页加 35,有人可能会想到用改变 offset 的方法来获得所有页面数据,我试过,会报错,无法找到页面,不知道是我方法不对还是怎么滴,有兴趣的可以试一下这里我们采取另一种方法来获取所有页面数据.

观察最后一行有个文本是下一页,而且 class='zbtn znxt',前面显示页面数据的class='zpgi'.一次我们可以通过class='zbtn znxt'来获取所有数据信息.

> 注意:使用webdrive.Chrome()会打开浏览器,每跳一个页面就会打开浏览器一次.我们可以将其换成chrome\_options = webdriver.ChromeOptions() chrome\_options.add\_argument('--headless') browser = webdriver.Chrome(chrome\_options=chrome\_options),它把网站加载到内存并执行页面上的JavaScript，但是它不会向用户展示网页的图形界面.

**完整代码**

``````````
from selenium import webdriverimport csvchrome_options = webdriver.ChromeOptions()chrome_options.add_argument('--headless')browser = webdriver.Chrome(chrome_options=chrome_options)csv_file = open('playlist.csv', 'w', newline='', encoding='utf-8')writer = csv.writer(csv_file)writer.writerow(['标题', '播放数', '链接'])url = 'http://music.163.com/#/discover/playlist/?order=hot&cat=%E5%85%A8%E9%83%A8&limit=35&offset=0'while url != 'javascript:void(0)': browser.get(url) browser.switch_to.frame('contentFrame') data = browser.find_element_by_id('m-pl-container').find_elements_by_tag_name('li') for i in range(len(data)): nb = data[i].find_element_by_class_name('nb').text if '万' in nb and int(nb.split('万')[0]) > 1000: msk = data[i].find_element_by_css_selector('a.msk') writer.writerow([msk.get_attribute('title'), nb, msk.get_attribute('href')]) url = browser.find_element_by_css_selector('a.zbtn.znxt').get_attribute('href')csv_file.close()
``````````


[python]: /pro/os/crawler/IRBE-IFIE-2IMN.jpg
[python 1]: /pro/os/crawler/VYIU-IZIE-6FAU.jpg
[python 2]: /pro/os/crawler/A2AV-NQ3I-IQ7N.jpg
 *  **原文作者：** waspvae
 *  **原文链接：** https://www.toutiao.com/item/6566986475332698631/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。