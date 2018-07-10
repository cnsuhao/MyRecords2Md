---
title: Spring-Boot前后端分离开发实战
date: 2018-04-22 16:08:26
categories: "开发"
tags:
	- CSS
	- jQuery
	- 技术
	- 软件
	- JSON

---

# 公司开发团队中，前端不懂后台，后端不懂前台，项目如何才能进行？ #

在这种情况下，前后端分离就显得非常重要了。这样的开发模式下，只要把restful接口设计好,大家就能各司其职进行开发了。那么Spring-Boot快速构建Web应用的时候如何前后端分离呢，今天就给大家分享个实战例子。先上效果图。

![Spring-Boot前后端分离开发实战][Spring-Boot]

中文首页

![Spring-Boot前后端分离开发实战][Spring-Boot 1]

中文登录成功页面

![Spring-Boot前后端分离开发实战][Spring-Boot 2]

中文登录失败页面

![Spring-Boot前后端分离开发实战][Spring-Boot 3]

英文首页

![Spring-Boot前后端分离开发实战][Spring-Boot 4]

英文登录失败页面

1、首先建立一个Spring-Boot项目，先给出一个完整的项目目录，然后再解释

![Spring-Boot前后端分离开发实战][Spring-Boot 5]

2、后端部分的话比较简单，就定义了一个登录restful接口，如果用户名和密码都是admin就返回登录成功JSON、否则返回失败JSON


![Spring-Boot前后端分离开发实战][Spring-Boot 6]

3、前端部分的话，采用AngularJS框架，目录比较多

public Spring-Boot默认的前端资源路径


index.html 首页

\--------css 存放CSS样式文件

\--------i18n 存放国际化JSON文件

\--------images 存放图片

\--------js

\----------------config 存放AngularJS 全局配置JS文件

\----------------controller 存放AngularJS 控制器JS文件

\----------------module 存放AngularJS 模块定义JS文件

\--------lib 存放第三方JS库，如AngularJS、JQuery

\--------view 存放视图

4、前端是个单页应用，所有的资源都在index.html中进行加载

![Spring-Boot前后端分离开发实战][Spring-Boot 7]

5、app.module.js定义了app模块，这里使用到了ngRoute（路由跳转）、translate（国际化）模块

![Spring-Boot前后端分离开发实战][Spring-Boot 8]

6、app.config.js进行全局配置，这里配置了路由跳转、国际化资源文件的位置


![Spring-Boot前后端分离开发实战][Spring-Boot 9]

7、app.controller.js书写了首页的业务逻辑，这里定义了语言转换下拉菜单、以及默认语言


![Spring-Boot前后端分离开发实战][Spring-Boot 10]

8、login.controller.js书写了登录页面的业务逻辑，获取用户名密码调用后台登录接口，成功后跳转到welcome页面并传递用户名称，如果登录失败则跳转到error页面

![Spring-Boot前后端分离开发实战][Spring-Boot 11]

9、loginSuccess.controller.js书写了登录成功后的业务，获取传递过来的username，更新到视图中

![Spring-Boot前后端分离开发实战][Spring-Boot 12]

10、loginFailed.controller.js书写了登录失败后的业务，设置5秒倒计时，跳转到登录页面

![Spring-Boot前后端分离开发实战][Spring-Boot 13]

11、国际化配置文件以及视图

![Spring-Boot前后端分离开发实战][Spring-Boot 14]

![Spring-Boot前后端分离开发实战][Spring-Boot 15]

![Spring-Boot前后端分离开发实战][Spring-Boot 16]

![Spring-Boot前后端分离开发实战][Spring-Boot 17]

![Spring-Boot前后端分离开发实战][Spring-Boot 18]

好了，今天分享到这里，喜欢的话来一波关注转发，谢谢大家


[Spring-Boot]: http://p1.pstatp.com/large/pgc-image/15243842926026138a2fea2
[Spring-Boot 1]: http://p3.pstatp.com/large/pgc-image/15243842992939059dde3cc
[Spring-Boot 2]: http://p1.pstatp.com/large/pgc-image/1524384300845ab4e88b888
[Spring-Boot 3]: http://p3.pstatp.com/large/pgc-image/15243843024795365305e8c
[Spring-Boot 4]: http://p3.pstatp.com/large/pgc-image/15243843050802386e4c0f7
[Spring-Boot 5]: http://p3.pstatp.com/large/pgc-image/15243819389153e7da91108
[Spring-Boot 6]: http://p9.pstatp.com/large/pgc-image/15243820189853416856a9c
[Spring-Boot 7]: http://p9.pstatp.com/large/pgc-image/1524382834312acba77381a
[Spring-Boot 8]: http://p1.pstatp.com/large/pgc-image/1524382951513d998d9a425
[Spring-Boot 9]: http://p3.pstatp.com/large/pgc-image/15243831440789a1454eee6
[Spring-Boot 10]: http://p3.pstatp.com/large/pgc-image/152438332109682f6fec14f
[Spring-Boot 11]: http://p3.pstatp.com/large/pgc-image/1524383556375ce66ee47a5
[Spring-Boot 12]: http://p3.pstatp.com/large/pgc-image/1524383724118d72e9bfde6
[Spring-Boot 13]: http://p1.pstatp.com/large/pgc-image/15243838115526a219a8dfc
[Spring-Boot 14]: http://p3.pstatp.com/large/pgc-image/152438405022838a21c7792
[Spring-Boot 15]: http://p3.pstatp.com/large/pgc-image/1524384057109244165b0bb
[Spring-Boot 16]: http://p3.pstatp.com/large/pgc-image/1524384057136e16bb6b1ba
[Spring-Boot 17]: http://p3.pstatp.com/large/pgc-image/1524384057125cf1b9e486b
[Spring-Boot 18]: http://p3.pstatp.com/large/pgc-image/15243840572882056803d44
 *  **原文作者：** Lucif堕落天使
 *  **原文链接：** https://www.toutiao.com/item/6547181600445039117/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。