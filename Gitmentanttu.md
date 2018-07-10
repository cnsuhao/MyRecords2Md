---
title: Gitment
date: 2017-08-29 20:52:05
categories: "开发"
tags:
	- 技术

---

Gitment 是作者[imsun][]实现的一款基于 GitHub Issues 的评论系统。支持在前端直接引入，不需要任何后端代码。可以在页面进行登录、查看、评论、点赞等操作，同时有完整的 Markdown / GFM 和代码高亮支持。尤为适合各种基于 GitHub Pages 的静态博客或项目页面。

本博客评论系统已迁移至 Gitment，参考作者的介绍部署成功，不过这里补充详细点，方便新手入门。

### 1、注册 OAuth Application ###

通过地址[传送门][Link 1]申请配置，注册一个新的 OAuth Application，其他内容可以随意填写，但要确保填入正确的 callback URL（一般是评论页面对应的域名，如 https://anttutu.github.io）。![JFNR-3IEM-AZE3.jpg][]创建成功后，你会得到一个 client ID 和一个 client secret，这个将被用于之后的用户登录。![RR3Y-ANAJ-YEB2.jpg][]

### 2、引入 Gitment ###

将下面的代码添加到你的页面：

``````````
<div id="container"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<script>
var gitment = new Gitment({
  id: '页面 ID', // 可选。默认为 location.href  比如我本人的就删除了
  owner: '你的 GitHub Name',              //比如我的叫anTtutu
  repo: '存储评论的 repo',                 //比如我的叫anTtutu.github.io
  oauth: {
    client_id: '你的 client ID',          //比如我的828***********
    client_secret: '你的 client secret',  //比如我的49e************************
  },
})
gitment.render('container')
</script>
``````````

## 为了灵活，我在\_config.yml中配置好全局参数： ##

![YYMI-EFVI-ERRF.jpg][]

### 3、初始化评论 ###

页面发布后，你需要访问页面并使用你的 GitHub 账号登录（请确保你的账号是第二步所填 repo 的 owner），点击初始化按钮。

之后其他用户即可在该页面发表评论

## 初始化：点击下初始化即可 ##

![FRUR-N2QN-MIER.jpg][]

## 正常： ##

![ZFY3-2E6J-FYUM.jpg][]![AUEE-REQQ-U2IQ.jpg][]

## 异常：通常是repo或者owner配置不对，请细心检测Error：Not Found 图1。还有本地调测会出现callback不对提示Error: Comments Not Initialized 图2 ##

![BJJJ-AFNY-3AVV.jpg][]![RRFM-RZEQ-67VA.jpg][]




[imsun]: https://imsun.net/posts/gitment-introduction/
[Link 1]: https://github.com/settings/applications/new
[JFNR-3IEM-AZE3.jpg]: /pro/os/crawler/JFNR-3IEM-AZE3.jpg
[RR3Y-ANAJ-YEB2.jpg]: /pro/os/crawler/RR3Y-ANAJ-YEB2.jpg
[YYMI-EFVI-ERRF.jpg]: /pro/os/crawler/YYMI-EFVI-ERRF.jpg
[FRUR-N2QN-MIER.jpg]: /pro/os/crawler/FRUR-N2QN-MIER.jpg
[ZFY3-2E6J-FYUM.jpg]: /pro/os/crawler/ZFY3-2E6J-FYUM.jpg
[AUEE-REQQ-U2IQ.jpg]: /pro/os/crawler/AUEE-REQQ-U2IQ.jpg
[BJJJ-AFNY-3AVV.jpg]: /pro/os/crawler/BJJJ-AFNY-3AVV.jpg
[RRFM-RZEQ-67VA.jpg]: /pro/os/crawler/RRFM-RZEQ-67VA.jpg
 *  **原文作者：** anttu
 *  **原文链接：** https://blog.csdn.net/anttu/article/details/77688004
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。