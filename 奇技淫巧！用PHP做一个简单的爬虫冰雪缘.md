---
title: 奇技淫巧！用PHP做一个简单的爬虫
date: 2017-11-05 01:38:21
categories: "开发"
tags:
	- 网络爬虫
	- 程序员
	- MySQL
	- 编程语言
	- PHP

---

# 随着网络的发展，各种奇技淫巧神代码养活了一大群码畜、码农、码神……但是，爬虫永远是最有用的神器。 #

接下开，小编直播爬虫的开发，编程语言是PHP。小编以前已经上传了2个开源爬虫库，现在小编用这两个爬虫库做个示范。  


![奇技淫巧！用PHP做一个简单的爬虫][PHP]

下载

    phpQuery.php

    QueryList.php

这两个库，在下载个数据库辅助库：

    Medoo.php

新建文件夹，按照下图排放：

![奇技淫巧！用PHP做一个简单的爬虫][PHP 1]

Medoo是一个数据库处理库，这不是我们介绍的重点，我们用它吧采集的数据插入数据库中。  


我们实例化一个Medoo，删除Medoo里的空间命名（为了简单）。

    $database= new medoo(['database_type' => 'mysql','database_name' => 'craw','server' => 'localhost','username' => 'root','password' => 'root','charset' => 'utf8']);

现在，写一个函数，是用来储存采集结果的。

    // 插入数据示例function insert($database, $url){$database->insert('list', ['url' => $url]);}

然后引入所有文件：  


    require_once 'class/Medoo.php';require_once 'mysql.php';require_once 'class/phpQuery.php';require_once 'class/QueryList.php';require_once 'function/insert.php';use QLQueryList;

假如我要采集这个网站，月薪范围大于2-3W的职位，我们发现能直接找到指向2-3W条件的URL，这个URL就是我们想要的地址了。

![奇技淫巧！用PHP做一个简单的爬虫][PHP 2]

要采集的DOM是：

一个div的class是el，选择第二个到结束的该div。

div里的P标签，再进一步，到a标签取出href属性就是我们要的详情页地址。于是，我们可以写出下列规则：

    '.el:gt(2)>p>span>a','href'

![奇技淫巧！用PHP做一个简单的爬虫][PHP 3]

我们打印一下结果：  


![奇技淫巧！用PHP做一个简单的爬虫][PHP 4]

果然返回了采集结果！  


现在，我们储存数据吧，按照下面两图操作。

![奇技淫巧！用PHP做一个简单的爬虫][PHP 5]

![奇技淫巧！用PHP做一个简单的爬虫][PHP 6]

现在，小编开始储存数据了……  


![奇技淫巧！用PHP做一个简单的爬虫][PHP 7]

运行一下：  


![奇技淫巧！用PHP做一个简单的爬虫][PHP 8]

看看数据库：

![奇技淫巧！用PHP做一个简单的爬虫][PHP 9]

采集完成，对内容的采集也是一样的。请关注小编，一直活跃的码畜。您的关注是对我们的鼓励~晚安！


[PHP]: /pro/os/crawler/EEQB-I3MU-VZEJ.jpg
[PHP 1]: /pro/os/crawler/RMVY-EB6F-E2YV.jpg
[PHP 2]: /pro/os/crawler/FIQZ-MRUM-BJEJ.jpg
[PHP 3]: /pro/os/crawler/3IVF-NU6Z-6ZN2.jpg
[PHP 4]: /pro/os/crawler/7RJY-V3YQ-NZAJ.jpg
[PHP 5]: /pro/os/crawler/RMRJ-JUNN-BANF.jpg
[PHP 6]: /pro/os/crawler/Y32I-QEFQ-QAQR.jpg
[PHP 7]: /pro/os/crawler/2AMA-UYBM-RQEY.jpg
[PHP 8]: /pro/os/crawler/7RA3-ENR2-QQQV.jpg
[PHP 9]: /pro/os/crawler/QZY3-AQJE-AJZF.jpg
 *  **原文作者：** 冰雪缘
 *  **原文链接：** https://www.toutiao.com/item/6484236834976039437/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。