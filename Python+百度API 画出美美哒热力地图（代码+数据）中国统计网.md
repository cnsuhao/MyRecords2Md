---
title: Python+百度API 画出美美哒热力地图（代码+数据）
date: 2018-01-03 20:44:10
categories: "开发"
tags:
	- 技术

---

之前在写葡萄酒数据分析那篇文章时

曾想过做一个葡萄酒分布的地区热力图

当时也没有搞定

在写完那篇文章之后

闲来无事

在网上查阅了很多技术博客

结合自己的摸索实践

摸索了一下午

终于完成了

当时真的很开心

# **数据准备** #

在国家统计局网站copy到2015年各城市的房地产开发投资额数据：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API]

把数据保存在本地，存成csv格式

百度API免费申请

打开网址：http://lbsyun.baidu.com/

注册好，按照下图操作：

点击 功能与服务----地图

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 1]

点击创建应用：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 2]

然后自己取个名字，选择浏览器端，白名单输入星号：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 3]

点击提交

你就有了一个应用了

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 4]

# **城市转换为经纬度** #

我们打开百度地图的开放平台

http://developer.baidu.com/map/jsdemo.htm\#c1\_15

在左侧找到添加热力图-----点击运行-----点击显示热力图-----就能看到热力图啦~

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 5]

这是百度API所能提供的热力图类型

我们注意到所给的这段代码：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 6]

我们将**您的密钥**换成刚才我们申请得到的那串数字

也就是：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 7]

然后在本地新建一个.html文件（可以通过创建文本文件，然后将后缀改为.html达到，如果你的文本文件不显示后缀，请百度解决~）

我们将加入了自己申请的密钥的网页代码全部复制下，粘贴到本地创建的 .html 文件中

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 8]

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 9]

复制的代码中的示例热力图的位置不是我们想要的

我们想要的是我们的数据生成热力图

打开Python

``````````
import jsonfrom urllib.request import urlopen, quoteimport requests,csvimport pandas as pd #导入这些库后边都要用到def getlnglat(address): url = 'http://api.map.baidu.com/geocoder/v2/' output = 'json' ak = '替换成你申请的密钥！！！' add = quote(address) #由于本文城市变量为中文，为防止乱码，先用quote进行编码 uri = url + '?' + 'address=' + add + '&output=' + output + '&ak=' + ak req = urlopen(uri) res = req.read().decode() #将其他编码的字符串解码成unicode temp = json.loads(res) #对json数据进行解析 return temp
``````````

``````````
file = open(r'E:房地产开发投资额2015.json','w') #建立json数据文件with open(r'E:房地产开发投资额2015.csv', 'r') as csvfile: #打开csv reader = csv.reader(csvfile) for line in reader: #读取csv里的数据 # 忽略第一行 if reader.line_num == 1: #由于第一行为变量名称，故忽略掉 continue # line是个list，取得所有需要的值 b = line[0].strip() #将第一列city读取出来并清除不需要字符 c = line[1].strip()#将第二列price读取出来并清除不需要字符 lng = getlnglat(b)['result']['location']['lng'] #采用构造的函数来获取经度 lat = getlnglat(b)['result']['location']['lat'] #获取纬度 str_temp = '{"lat":' + str(lat) + ',"lng":' + str(lng) + ',"count":' + str(c) +'},' #print(str_temp) #也可以通过打印出来，把数据copy到百度热力地图api的相应位置上 file.write(str_temp) #写入文档file.close() #保存
``````````

参考文献：https://www.jianshu.com/p/773ff5f08a2c

运行完成之后，本地会出现一个json文件

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 10]

打开可以看到：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 11]

是一长串json数据，我们全选这些数据，复制替换掉本地的html文件中的var经纬度部分

原本是这样的：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 12]

替换完成后是这样的：

（我是用notepad++以编辑的方式打开的本地html文件）

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 13]

保存之后，以网页浏览的方式打开本地的html

使用鼠标滚轮放大缩小到可以看见全国视野，然后点击左下角显示热力图即可看到~

你出现的图是这样的：

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 14]

因为原始数据默认为20和100，只要超过100的值显示都一样，而且没有渐变辐射的效果

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 15]

可以修改热力点辐射半径（辐射范围大小）和最大值

我们数据的price最大是北京的4177，所以我设置为4000，保存

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 16]

再次打开操作就能看到美美哒热力图啦~

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 17]

你可以拿这个方法去做其他热力图啦~比如房价热力图，GDP热力图等等~

![Python+百度API 画出美美哒热力地图（代码+数据）][Python_API 18]

End.

运行人员：中国统计网小编

中国统计网，是国内最早的大数据学习网站

//www.itongji.cn


[Python_API]: /pro/os/crawler/6ZNU-FMQZ-V3EI.jpg
[Python_API 1]: /pro/os/crawler/2MAU-F2YE-BIFR.jpg
[Python_API 2]: /pro/os/crawler/3M3Q-MARZ-JZEE.jpg
[Python_API 3]: /pro/os/crawler/3MJN-Z2V3-YRER.jpg
[Python_API 4]: /pro/os/crawler/ZAJB-AEJM-RBNI.jpg
[Python_API 5]: /pro/os/crawler/ER7F-IYUE-N6ZY.jpg
[Python_API 6]: /pro/os/crawler/UI2M-MJEZ-FZAV.jpg
[Python_API 7]: /pro/os/crawler/MJUN-EYBQ-AV7B.jpg
[Python_API 8]: /pro/os/crawler/UFE3-2ENY-A7JM.jpg
[Python_API 9]: /pro/os/crawler/VUBF-BI7V-ZJYM.jpg
[Python_API 10]: /pro/os/crawler/3ARU-VEAA-7NAU.jpg
[Python_API 11]: /pro/os/crawler/JM3U-UBBZ-JN22.jpg
[Python_API 12]: /pro/os/crawler/2IEM-2MAB-EAZB.jpg
[Python_API 13]: /pro/os/crawler/IBNY-NZFZ-FEMY.jpg
[Python_API 14]: /pro/os/crawler/NUJ3-AMIF-6ZQ3.jpg
[Python_API 15]: /pro/os/crawler/FERE-V2E6-NM63.jpg
[Python_API 16]: /pro/os/crawler/RUJV-Q2UZ-RQ7F.jpg
[Python_API 17]: /pro/os/crawler/V3EV-JQAI-IZ2Q.jpg
[Python_API 18]: /pro/os/crawler/NRIQ-FJUY-UMEF.jpg
 *  **原文作者：** 中国统计网
 *  **原文链接：** https://www.toutiao.com/item/6506652855087153677/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。