---
title: 打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波
date: 2018-06-27 23:54:32
categories: "开发"
tags:
	- 二手房
	- 购房
	- 编程语言
	- HTML
	- Python

---

1.前言

本人是个学生党，在过两年就要研究生毕业了，面临着找工作，相信很多人也面临或者经历过工作，定居租房买房之类的

在此，我们来采集一下上海在售的二手房信息，有人想问，为啥不采集新房？快醒醒吧，新房可远观而不可亵玩焉，一般人都买不起，看的只会心情不好，hhhh

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python]

当然，二手房估计你也买不起！咱们拿数据说话！

2.观察网站结构

以本人所在的城市上海为例，走在上海的大街小巷，你会看到很多做房产中介的，最常见的就是链家了~

我们进一下链家的上海二手房页面：上海二手房|上海二手房出售|最新上海二手房信息 - 上海链家网链家二手房交易&utm\_content=链家二手房&utm\_campaign=品牌词

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 1]

有81508套二手房源在出售，这么多！

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 2]

3.寻找需要爬取信息

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 3]

感觉这些红色框的我都想要，但是感觉还是不够全面，我们点击进去看看详细信息。

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 4]

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 5]

这里面的信息挺全的，当然，我根据需要的数据（可能之后分析需要用到）来选择爬取的数据

分析网页结构在我之前的文章里有写到，就不赘述了

传送门：

Python网络爬虫爬取智联招聘职位：Python网络爬虫爬取智联招聘职位 - 天善智能：专注于商业智能BI和数据分析、大数据领域的垂直社区平台

爬取起点中文网月票榜前500名网络小说介绍：爬取起点中文网月票榜前500名网络小说介绍 - 天善智能：专注于商业智能BI和数据分析、大数据领域的垂直社区平台

4.撰写爬虫

``````````
#主要程序import requestsimport refrom bs4 import BeautifulSoupfrom fake_useragent import UserAgentua=UserAgent()#使用随机header，模拟人类headers1={'User-Agent': 'ua.random'}#使用随机header，模拟人类houseary=[]#建立空列表放房屋信息domain='http://sh.lianjia.com'#为了之后拼接子域名爬取详细信息for i in range(1,400):#爬取399页，想爬多少页直接修改替换掉400，不要超过总页数就好 res=requests.get('http://sh.lianjia.com/ershoufang/d'+str(i),headers=headers1)#爬取拼接域名 soup = BeautifulSoup(res.text,'html.parser')#使用html筛选器#print(soup)for j in range(0,29):#网站每页呈现30条数据，循环爬取 url1=soup.select('.prop-title a')[j]['href']#选中class=prop-title下的a标签里的第j个元素的href子域名内容 url=domain+url1#构造子域名 houseary.append(gethousedetail1(url,soup,j))#传入自编函数需要的参数def gethousedetail1(url,soup,j):#定义函数，目标获得子域名里的房屋详细信息 info={}#构造字典，作为之后的返回内容 s=soup.select('.info-col a')[1+3*j]#通过传入的j获取所在区的内容 pat='<a.*?>(.*?)</a>'#构造提取正则 info['所在区']=''.join(list(re.compile(pat).findall(str(s))))#使用join将提取的列表转为字符串 s1=soup.select('.info-col a')[0+3*j]#[0].text.strip() pat1='<span.*?>(.*?)</span>' info['具体地点']=''.join(list(re.compile(pat1).findall(str(s1)))) s2=soup.select('.info-col a')[2+3*j]#[0].text.strip() pat2='<a.*?>(.*?)</a>' info['位置']=''.join(list(re.compile(pat2).findall(str(s2)))) q=requests.get(url)#使用子域名 soup=BeautifulSoup(q.text,'html.parser')#提取子域名内容,即页面详细信息for dd in soup.select('.content li'):#提取class=content标签下的li标签房屋信息 a=dd.get_text(strip=True)#推荐的去空格方法，比strip（）好用if '：' in a:#要有冒号的，用中文的冒号，因为网页中是中文  key,value=a.split('：')#根据冒号切分出键和值 info[key]=value info['总价']=soup.select('.bold')[0].text.strip()#提取总价信息return info#传回这一个页面的详细信息
``````````

写了详细注释，相信萌萌的你可以看懂~

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 6]

我们来看一下爬的结果：

``````````
houseary#看一下列表信息
``````````

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 7]

就是将每次爬取的信息做成dict依次添加在list中

接下来使用pandas神器~

``````````
import pandas#pandas大法好df=pandas.DataFrame(houseary)df
``````````

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 8]

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 9]

考虑到主程序写了双重for循环，函数里写了循环，所以时间复杂度是O（n^3）,对于一个算法，一般是不可以接受的，好吧，萌萌的我只能接受，如果你问我为什么，我只能说，我写不出低复杂度的了。。。爬了这1w+条数据用了我1小时时间。。。各位dalao如果有方法可以指点一下，之后我想学习多线程提高爬取速度~

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 10]

最后存到本地excel文件中

``````````
df.to_excel('house_lianjia.xlsx')
``````````

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 11]

5.结语

看到这价格是不是有句mmp想说

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 12]

之后会写一篇《Python数据采集和分析告诉你为何上海的二手房你都买不起！（二）》的数据分析和可视化的文章深入分析一下这次抓到的数据~敬请期待，么么哒

![打一辈子工感觉也在上海买不起一套二手房！Python强行分析了一波][Python 13]

**私信小编007即可获取数十套PDF书籍！**


[Python]: /pro/os/crawler/NUNM-VJI6-3MFB.jpg
[Python 1]: /pro/os/crawler/I2AM-Q3UF-Z26J.jpg
[Python 2]: /pro/os/crawler/UNBE-U2EY-B32M.jpg
[Python 3]: /pro/os/crawler/UBBV-FMQB-RYRE.jpg
[Python 4]: /pro/os/crawler/QIQI-Q2QA-VZR2.jpg
[Python 5]: /pro/os/crawler/6VF7-VEUN-ZJAY.jpg
[Python 6]: /pro/os/crawler/NRZY-AJNM-2YUR.jpg
[Python 7]: /pro/os/crawler/AE6F-6NMB-3M32.jpg
[Python 8]: /pro/os/crawler/QQZV-NNFJ-MVF2.jpg
[Python 9]: /pro/os/crawler/UAJB-YBAJ-6RB2.jpg
[Python 10]: /pro/os/crawler/QUYJ-7RE3-QNNJ.jpg
[Python 11]: /pro/os/crawler/AUUE-NJR2-ENYN.jpg
[Python 12]: /pro/os/crawler/AU3Q-M3EI-QN6N.jpg
[Python 13]: /pro/os/crawler/MZIU-Q3EY-REV3.jpg
 *  **原文作者：** 繁华落尽and曲终人散
 *  **原文链接：** https://www.toutiao.com/item/6571793336644928003/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。