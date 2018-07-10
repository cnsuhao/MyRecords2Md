---
title: Python爬虫 + 人脸检测 + 颜值检测 &#x3D; 知乎高颜值图片抓取
date: 2018-03-19 11:57:05
categories: "开发"
tags:
	- 网络爬虫
	- 软件
	- 编程语言
	- 人工智能
	- Python

---

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _]

**声明**：文中所有文字、图片以及相关外链中直接或间接、明示或暗示涉及性别、颜值分数等信息全部由相关人脸检测接口给出。无任何客观���，仅供参考。

# 1 数据源 #

知乎话题**『美女』**下所有问题中回答所出现的图片

# 2 抓取工具 #

Python 3，并使用第三方库 **Requests、lxml、AipFace**，代码共 **100 +** 行

# 3 必要环境 #

 *  Mac / Linux / Windows （Linux 没测过，理论上可以。Windows 之前较多反应出现异常，后查是 windows 对本地文件名中的字符做了限制，已使用正则过滤）
 *  无需登录知乎（即无需提供知乎帐号密码）
 *  人脸检测服务需要一个百度云帐号（即百度网盘 / 贴吧帐号）

# 4 人脸检测库 #

AipFace，由百度云 AI 开放平台提供，是一个可以进行人脸检测的 Python SDK。可以直接通过 HTTP 访问，免费使用。

`文档中心--百度AI:ai.baidu.com`。

# 5 检测过滤条件 #

 *  过滤所有未出现人脸图片（比如风景图、未露脸身材照等）
 *  过滤所有非女性（在抓取中，发现知乎男性图片基本是明星，故不考���；存在 AipFace 性别识别不准的情况）
 *  过滤所有非真实人物，比如动漫人物 （AipFace Human 置信度小于 0.6）
 *  过滤所有颜值评分较低图片（AipFace beauty 属性小于 45，为了节省存储空间；再次声明，AipFace 评分无任何客观性）

# 6 实现逻辑 #

 *  通过 Requests 发起 HTTP 请求，获取『美女』下的部分讨论列表
 *  通过 lxml 解析抓取到的每个讨论中 HTML，获取其中所有的 img 标签相应�� src 属性
 *  通过 Requests 发起 HTTP 请求，下载 src 属性指向图片（不考虑动图）
 *  通过 AipFace 请求对图片进行人脸检测
 *  判断是否检测到人脸，并使用 『4 检测过滤条件』过滤
 *  将过滤后的图片持久化到本地文件系统，文件名为 颜值 + 作者 + 问题名 + 序号
 *  返回第一步，继续

# 7 抓取结果 #

直接存放在文件夹中（angelababy 实力出境）。另外说句，目前抓下来的图片，除 baby 外，88 分是最高分。个人对其中的排序表示反对，老婆竟然不是最高分

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 1]

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 2]

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 3]

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 4]

# 8 代码 #

`#coding: utf-8`

`import time`

`import os`

`import re`

`import requests`

`from lxml import etree`

`from aip importAipFace`

`#百度云 人脸检测 申请信息`

`#唯一必须填的信息就这三行`

`APP_ID ="xxxxxxxx"`

`API_KEY ="xxxxxxxxxxxxxxxxxxxxxxxx"`

`SECRET_KEY ="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"`

`# 文件存放目录名，相对于当前目录`

`DIR ="image"`

`# 过滤颜值阈值，存储空间大的请随意`

`BEAUTY_THRESHOLD =45`

`#如果权限错误，浏览器中打开知乎，在开发者工具复制一个，无需登录`

`#建议最好换一个，因为不知道知乎的反爬虫策略，如果太多人用同一个，可能会影响程序运行`

`#如何替换该值下文有讲述`

`AUTHORIZATION ="oauth c3cef7c66a1843f8b3a9e6a1e3160e20"`

`#以下皆无需改动`

`#每次请求知乎的讨论列表长度，不建议设定太长，注意节操`

`LIMIT =5`

`#这是话题『美女』的 ID，其是『颜值』（20013528）的父话题`

`SOURCE ="19552207"`

`#爬虫假装下正常浏览器请求`

`USER_AGENT ="Mozilla/5.0 (Windows NT 5.1) AppleWebKit/534.55.3 (KHTML, like Gecko) Version/5.1.5 Safari/534.55.3"`

`#爬虫假装下正常浏览器请求`

`REFERER ="https://www.zhihu.com/topic/%s/newest"% SOURCE`

`#某话题下讨论列表请求 url`

`BASE_URL ="https://www.zhihu.com/api/v4/topics/%s/feeds/timeline_activity"`

`#初始请求 url 附带的请求参数`

`URL_QUERY ="?include=data%5B%3F%28target.type%3Dtopic_sticky_module%29%5D.target.data%5B%3F%28target.type%3Danswer%29%5D.target.content%2Crelationship.is_authorized%2Cis_author%2Cvoting%2Cis_thanked%2Cis_nothelp%3Bdata%5B%3F%28target.type%3Dtopic_sticky_module%29%5D.target.data%5B%3F%28target.type%3Danswer%29%5D.target.is_normal%2Ccomment_count%2Cvoteup_count%2Ccontent%2Crelevant_info%2Cexcerpt.author.badge%5B%3F%28type%3Dbest_answerer%29%5D.topics%3Bdata%5B%3F%28target.type%3Dtopic_sticky_module%29%5D.target.data%5B%3F%28target.type%3Darticle%29%5D.target.content%2Cvoteup_count%2Ccomment_count%2Cvoting%2Cauthor.badge%5B%3F%28type%3Dbest_answerer%29%5D.topics%3Bdata%5B%3F%28target.type%3Dtopic_sticky_module%29%5D.target.data%5B%3F%28target.type%3Dpeople%29%5D.target.answer_count%2Carticles_count%2Cgender%2Cfollower_count%2Cis_followed%2Cis_following%2Cbadge%5B%3F%28type%3Dbest_answerer%29%5D.topics%3Bdata%5B%3F%28target.type%3Danswer%29%5D.target.content%2Crelationship.is_authorized%2Cis_author%2Cvoting%2Cis_thanked%2Cis_nothelp%3Bdata%5B%3F%28target.type%3Danswer%29%5D.target.author.badge%5B%3F%28type%3Dbest_answerer%29%5D.topics%3Bdata%5B%3F%28target.type%3Darticle%29%5D.target.content%2Cauthor.badge%5B%3F%28type%3Dbest_answerer%29%5D.topics%3Bdata%5B%3F%28target.type%3Dquestion%29%5D.target.comment_count&limit="+ str(LIMIT)`

`#指定 url，获取对应原始内容 / 图片`

`def fetch_image(url):`

`try:`

`headers ={`

`"User-Agent": USER_AGENT,`

`"Referer": REFERER,`

`"authorization": AUTHORIZATION`

`}`

`s = requests.get(url, headers=headers)`

`exceptExceptionas e:`

`print("fetch last activities fail. "+ url)`

`raise e`

`return s.content`

`#指定 url，获取对应 JSON 返回 / 话题列表`

`def fetch_activities(url):`

`try:`

`headers ={`

`"User-Agent": USER_AGENT,`

`"Referer": REFERER,`

`"authorization": AUTHORIZATION`

`}`

`s = requests.get(url, headers=headers)`

`exceptExceptionas e:`

`print("fetch last activities fail. "+ url)`

`raise e`

`return s.json()`

`#处理返回的话题列表`

`def process_activities(datums, face_detective):`

`for data in datums["data"]:`

`target = data["target"]`

`if"content"notin target or"question"notin target or"author"notin target:`

`continue`

`#解析列表中每一个元素的内容`

`html = etree.HTML(target["content"])`

`seq =0`

`#question_url = target["question"]["url"]`

`question_title = target["question"]["title"]`

`author_name = target["author"]["name"]`

`#author_id = target["author"]["url_token"]`

`print("current answer: "+ question_title +" author: "+ author_name)`

`#获取所有图片地址`

`images = html.xpath("//img/@src")`

`for image in images:`

`ifnot image.startswith("http"):`

`continue`

`s = fetch_image(image)`

`#请求人脸检测服务`

`scores = face_detective(s)`

`for score in scores:``filename =("%d--"% score)+ author_name +"--"+ question_title +("--%d"% seq)+".jpg"`

`filename = re.sub(r'(?u)[^-w.]','_', filename)`

`#注意文件名的处理，不同平台的非法字符不一样，这里只做���简单处理，特别是 author_name / question_title 中的内容`

`seq = seq +1`

`with open(os.path.join(DIR, filename),"wb")as fd:`

`fd.write(s)`

`#人脸检测 免费，但有 QPS 限制`

`time.sleep(2)`

`ifnot datums["paging"]["is_end"]:`

`#获取后续讨论列表的请求 url`

`return datums["paging"]["next"]`

`else:`

`returnNone`

`def get_valid_filename(s):`

`s = str(s).strip().replace(' ','_')`

`return re.sub(r'(?u)[^-w.]','_', s)`

`def init_face_detective(app_id, api_key, secret_key):`

`client =AipFace(app_id, api_key, secret_key)`

`#人脸检测中，在响应中附带额外的字段。年龄 / 性别 / 颜值 / 质量`

`options ={"face_fields":"age,gender,beauty,qualities"}`

`def detective(image):`

`r = client.detect(image, options)`

`#如果没有检测到人脸`

`if r["result_num"]==0:`

`return[]`

`scores =[]`

`for face in r["result"]:`

`#人脸置信度太低`

`if face["face_probability"]<0.6:`

`continue`

`#真实人脸置信度太低`

`if face["qualities"]["type"]["human"]<0.6:`

`continue`

`#颜值低于阈值`

`if face["beauty"]< BEAUTY_THRESHOLD:`

`continue`

`#性别非女性`

`if face["gender"]!="female":`

`continue`

`scores.append(face["beauty"])`

`return scores`

`return detective`

`def init_env():`

`ifnot os.path.exists(DIR):`

`os.makedirs(DIR)`

`init_env()`

`face_detective = init_face_detective(APP_ID, API_KEY, SECRET_KEY)`

`url = BASE_URL % SOURCE + URL_QUERY`

`while url isnotNone:`

`print("current url: "+ url)`

`datums = fetch_activities(url)`

`url = process_activities(datums, face_detective)`

`#注意节操，爬虫休息间隔不要调小`

`time.sleep(5)`

`# vim: set ts=4 sw=4 sts=4 tw=100 et:`

# 9 运行准备 #

 *  安装 Python 3，Download Python
 *  安装 requests、lxml、baidu-aip 库，都可以通过 **pip** 安装，一行命令
 *  申请百度云检测服务，免费。人脸识别-百度AI

**要求登录，百度帐号可以直接使用（贴吧/网盘通用），没有���能注册**

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 5]

**点击创建应用**

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 6]

**随便填下**

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 7]

**将 AppID ApiKek SecretKey 填写到 代码 中**

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 8]

 *  （可选）配置自定义信息，如图片存储目录、颜值阈值、人脸置信度等
 *  （可选）若请求知乎失败，返回如下。需更改 AUTHORIZATION，可从开发者工具中获取（如下图）

``````````
{ "error": { "message": "ZERR_NO_AUTH_TOKEN", "code": 100, "name": "AuthenticationInvalidRequest" } }
``````````

**Chrome 浏览器；找一个知乎链接点进去，打开开发者工具，查看 HTTP 请求 header；无需登录**

![Python爬虫 + 人脸检测 + 颜值检测 = 知乎高颜值图片抓取][Python_ _ _ _ _ _ 9]

 *  运行 ^\*^

# 10 结语 #

 *  因是人脸检测，所以可能有些福利会被筛掉。百度图像识别 API 还有一个叫做色情识别。这个 API 可以识别不可描述以及性感指数程度，可以用这个 API 来找福利（逃 `图像审核-百度AI:cloud.baidu.com`
 *  如果实在不想申请百度云服务，可以直接把人脸检测部分注释掉，当做单纯的爬虫使用 人脸检测部���可以替换成其他厂商服务或者本地模型，这里用百度云是因为它不要钱
 *  抓了几千张照片，效果还是挺不错的。有兴趣可以把代码贴下来跑跑试试
 *  这边文章只是基础爬虫 + 数据过滤来获取较高质量数据的示例，希望有兴趣者可以 run 下，代码里有很多地方可以很容易的修改，从最简单的数据源话题变更、抓取数据字段增加和删除到图片过滤条件修改都很容易。如果再稍微花费时间，变更为抓取某人动态（比如轮子哥，数据质量很高）、探索 HTTP 请求中哪些 header 和 query 是必要的，文中代码都只需要非常局部性的修改。至于人脸探测，或者其他机器学习接口，可以提供非常多的功能用于数据过滤，但哪些过滤是具备高可靠性，可信赖的且具备可用性，这个大概是经验和反复试验，这就是额外的话题了；顺便希望大家有良好的编码习惯
 *  最后再次声明，颜值得分以及性别过滤存在 bad case，请勿认真对待。


[Python_ _ _ _ _ _]: /pro/os/crawler/VBYM-UYME-NIUU.jpg
[Python_ _ _ _ _ _ 1]: /pro/os/crawler/JYFV-I2AM-YJIV.jpg
[Python_ _ _ _ _ _ 2]: /pro/os/crawler/BMUN-YMQU-VZZR.jpg
[Python_ _ _ _ _ _ 3]: /pro/os/crawler/UFJY-ZUJF-R3M2.jpg
[Python_ _ _ _ _ _ 4]: /pro/os/crawler/A32U-YNYV-FFBF.jpg
[Python_ _ _ _ _ _ 5]: /pro/os/crawler/VIIM-AVZM-FQAV.jpg
[Python_ _ _ _ _ _ 6]: /pro/os/crawler/YRQ7-JVI2-EJFV.jpg
[Python_ _ _ _ _ _ 7]: /pro/os/crawler/UJNF-RZQJ-NFYJ.jpg
[Python_ _ _ _ _ _ 8]: http://p3.pstatp.com/large/pgc-image/1521431300574d93a937ab0
[Python_ _ _ _ _ _ 9]: /pro/os/crawler/MYQA-NAZN-6ZQM.jpg
 *  **原文作者：** 豌豆上的主公
 *  **原文链接：** https://www.toutiao.com/item/6534499933242786307/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。