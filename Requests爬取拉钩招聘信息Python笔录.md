---
title: Requests爬取拉钩招聘信息
date: 2018-03-24 05:16:07
categories: "开发"
tags:
	- Windows NT
	- Mozilla
	- JSON
	- Windows
	- Gecko

---

\#coding=utf-8

import requests,json

from pymongo import MongoClient

\#数据库建立

c = MongoClient(host='127.0.0.1',port=27017)

db = c\['lagou'\]

collection = db\['lagoudata'\]

def lagou(company):

\#代理IP

s = requests.session()

s.verxify = True

s.proxies = '121.232.146.179:8081'

url = 'https://www.lagou.com/jobs/positionAjax.json'

handers=\{

'Referer':'https://www.lagou.com/jobs/list\_%s'%company,

'User-Agent':'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36'

\}

\#爬取三页数据

for page in xrange(1,4):

data=\{'pn': page\}

res = requests.post(url=url,data=data, headers=handers).content

two=None

three=None

four=None

five=None

datajson = json.loads(res)

if datajson.has\_key('content'):

two = datajson\['content'\]

if two.has\_key('positionResult'):

three = two\['positionResult'\]

if three.has\_key('result'):

four = three\['result'\]

for g in xrange(len(four)):

five = four\[g\]\['positionName'\]

print five

\# 存入数据库中

collection.insert(\{'charles': five\})

print ('----------第%s页----------'%page)

if \_\_name\_\_ == '\_\_main\_\_':

company = raw\_input('请输入您要查找的公司:')

lagou(company)
 *  **原文作者：** Python笔录
 *  **原文链接：** https://www.toutiao.com/item/6535757943764156935/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。