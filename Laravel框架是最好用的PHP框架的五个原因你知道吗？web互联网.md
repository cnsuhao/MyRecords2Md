---
title: Laravel框架是最好用的PHP框架的五个原因你知道吗？
date: 2018-04-22 11:13:40
categories: "开发"
tags:
	- 脚本语言
	- 程序员
	- Nginx
	- 编程语言
	- PHP

---

> PHP框架众多，比如ThinkPHP、Yii和Laravel框架，但是用了这么多框架，才发现Laravel框架是最好用的，下面我们来分析一下为什么会这样。

**搭建非常简单**

![Laravel框架是最好用的PHP框架的五个原因你知道吗？][Laravel_PHP]

我们打开LV框架的官方文档，发现它的搭建步骤写得非常清楚，并且在APACHE和NGINX的里面的配置也是写得非常清楚，我们仅仅只需要几分钟的时间就能够创建一个基于LV框架的项目。


**中间件**

![Laravel框架是最好用的PHP框架的五个原因你知道吗？][Laravel_PHP 1]

所谓中间件，就是所有设置了中间件的路由请求都会从中间件经过，这有什么用呢？我们可以利用中间件来完成用户登录的判断，新增一些请求参数等。


**告别反复修改crontab的时代**

![Laravel框架是最好用的PHP框架的五个原因你知道吗？][Laravel_PHP 2]

我们的项目经常需要在CLI模式下执行一些统计类的需要，以前，我们经常在crontab里面添加多条执行任务，比如晚上1点执行a.php脚本，2点执行b.php脚本，3点又执行c.php脚本，如果有需要，总是反复修改crontab文件，现在好了，有了LV框架，我们仅仅添加一条就够了，其他的事情都是LV框架来控制。


**数据库的操作非常丰富**


![Laravel框架是最好用的PHP框架的五个原因你知道吗？][Laravel_PHP 3]

在LV框架中，提供了很多种操作数据库的方法，有原生SQL的，有半封装的，有全封装的，总之适合每一类PHP程序员。


**引入了.env文件**

![Laravel框架是最好用的PHP框架的五个原因你知道吗？][Laravel_PHP 4]

有了配置文件还不够，还引入了先进的.env文件，利用这个.env文件，我们能够实现本地开发环境、测试环境、生产环境的配置文件分离，再与不用反反复复修改各种环境下面的配置文件了。


> 喜欢我就关注我，一个程序员老司机，和你分享编程、运营、需求等等经验和趣事。


[Laravel_PHP]: http://p3.pstatp.com/large/pgc-image/1524366691649aaaebec047
[Laravel_PHP 1]: http://p3.pstatp.com/large/pgc-image/1524366708086fba57e1b4e
[Laravel_PHP 2]: http://p1.pstatp.com/large/pgc-image/152436672141345e3050212
[Laravel_PHP 3]: http://p3.pstatp.com/large/pgc-image/15243667397224196beccb4
[Laravel_PHP 4]: http://p3.pstatp.com/large/pgc-image/152436675651081de49da87
 *  **原文作者：** web互联网
 *  **原文链接：** https://www.toutiao.com/item/6547105640697823747/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。