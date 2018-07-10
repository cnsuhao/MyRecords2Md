---
title: 京东抽奖 chrome浏览器测水教程
date: 2017-05-22 17:49:36
categories: "开发"
tags:
	- 软件
	- Chrome
	- 京东
	- 电子商务
	- Firefox

---

首先，这个演示是用chrome浏览器来做得，新版的IE和火狐也可以进行类似操作。  


这个测水只能显示什么时候有水，需要你盯着手动，不能自动抽。不喜欢得可以去找大牛买软件。

下边是图文教程，上边也有视频，可以对照操作。

首先，我们用chrome打开京东抽奖页面，打开页面源代码，找到这个这个抽奖的抽奖码

![京东抽奖 chrome浏览器测水教程][chrome]

代码比较长，怎么找到码，可以搜lotterycode，后边跟得就是。

之后这是先前准备好得程序代码，如下

setInterval(function()\{

console.clear();

var lotteryCode = 'c502caa2-80f2-4105-b022-3958520e2d7e';// 抽奖代码

var sh = 3;// 最新中奖数量

$.ajax(\{

url: 'https://ls-activity.jd.com/lotteryApi/getWinnerList.action?lotteryCode='+lotteryCode,

cache: false,

dataType: "jsonp",

success: function (data) \{

if(data.responseCode === '0000')\{

var pz = data.data;

if(pz.length)\{

for(var i = 0; i < sh; i++)\{

console.info(pz\[i\].prizeName+' - '+pz\[i\].userPin+' - '+pz\[i\].winDate);

\}

\}

var de = pz\[0\].winDate;

var ms = new Date(de) - new Date();

if(30000 >= ms >= 1000)\{

console.info('30秒内有中奖');

\}

\}

\},

error: function (data) \{

console.info('error');

\}

\});

\},2000);

我们需要将里边的 var lotteryCode = 'c502caa2-80f2-4105-b022-3958520e2d7e';// 抽奖代码

修改成我们要抽奖页面的抽奖码。主要标点别掉了。

![京东抽奖 chrome浏览器测水教程][chrome 1]

修改之后，我们复制全部代码。

回到我们之前打开得抽奖页面，按F12。掉出控制台

选择console 把上边得代码粘上去，回车就可以看到实时中奖信息了

![京东抽奖 chrome浏览器测水教程][chrome 2]


[chrome]: /pro/os/crawler/IAFM-FFZE-RAVF.jpg
[chrome 1]: /pro/os/crawler/RJFJ-6VAE-NFVY.jpg
[chrome 2]: /pro/os/crawler/YFYE-BMNB-YA3I.jpg
 *  **原文作者：** 幽草涧
 *  **原文链接：** https://www.toutiao.com/item/6422894137715982850/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。