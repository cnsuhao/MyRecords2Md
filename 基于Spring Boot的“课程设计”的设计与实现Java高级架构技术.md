---
title: 基于Spring Boot的“课程设计”的设计与实现
date: 2018-06-19 20:33:33
categories: "开发"
tags:
	- Nginx
	- NoSQL
	- MySQL
	- 编程语言
	- Redis

---

# **这是一个集电影，音乐和书籍于一体的Java web应用** #

**Java 1.8**

**框架：使用Spring Boot 集成Spring,Spring MVC,MyBatis(前期),Spring Data(后期)**

**数据库:MySQL 5.6**

**缓存:Redis 4.0**

**版本控制:Maven 3.5**

**页面解析框架:Thymeleaf**

**负载均衡:Nginx - 端口80**

**服务器:Tomcat 端口8080和8181(可以使用单个tomcat)**

**PS：音乐来源-网易云；电影来源-豆瓣、猫眼；书籍来源-豆瓣**

==================================================

**项目结构**

``````````
com.wsk.movie aspect:切面应用 bean:回显的实体类 celebrity:json影人条目信息 maoyan:猫眼 cinema:json单个电影院信息 cinemas:json多个电影院信息 movie:json电影信息 config:spring启动加载配置 controller:链接控制 webSocket:websocket相关配置和实现 dao:Mybatis接口 error:自定义异常处理 music:网易云音乐 bean:网易云音乐json解析类 entity:数据库实体类 service:操作数据库 thread:线程相关 pojo:电影相关的数据库实体 redis:redis操作类 impl:接口的实现 service:电影相关的服务操作 impl:接口的实现 session:session存活时间配置 springdata:网易云音乐spring data操作 entity:网易云音乐的数据库实体类 task:自定义的定时器 entity:数据库实体类 runnable:任务 service:数据库相关操作 tool:工具类 token:token生成器 tool:工具类 bean:百度图片识别json结果 write:文件读写操作 resources mapping:mybatis相关的xml文件 static:静态资源文件 css:样式 image:本地图片 js:JAVASCRIPT templates:页面 forget:忘记密码 hot:热门电影 information:个人相关信息详情 movie:电影相关信息 registered:注册 setting:设置12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
``````````

**1. 系统结构**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot]

**2. 业务流程**

**客户端**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 1]

**管理员**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 2]

**4. 数据库**

（1） 数据库表汇总

数据库表汇总

**名称表名注释**管理员操作记录表adminaction记录管理员操作管理员信息表admininformation记录管理员信息书籍表book记录书籍、图书户收藏表collectioncritic记录用户收藏的信息说说评论表commentcritic记录说说的评论举报信息表critic\_report记录举报信息点赞信息表goodcritic记录说说的点赞情况积分来源表integralsource记录积分的来源通讯信息表message记录用户之间的通讯电影名称表moviename记录电影名好友表myfriends记录用户之间的好友关系任务表mytask记录后台定时任务任务错误信息表mytaskerror记录后台任务错误信息任务日志表mytasklog记录后台任务运行情况说说表publishcritic记录用户发布的说说用户信息表userinformation记录用户的信息用户信誉积分表userintegral记录用户的信誉积分用户等级表userlevel记录用户的等级用户密码表userpassword记录用户的密码用户二维码表userqrcode记录用户的二维码音乐专辑表wangyialbum记录音乐专辑音乐信息表wangyimusic记录音乐信息音乐歌手表wangyisinger记录歌手信息

**5. 部分流程图**

**5.1 用户登录**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 3]

**5.2 发表说说**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 4]

**5.3 欣赏电影，聆听音乐，阅读书籍**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 5]

**5.4 用户信息互动**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 6]

**5.5 管理管理用户，说说和举报审核**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 7]

**6 具体实现细节**

**6.1 项目技术架构**

**6.2 登录界面的实现**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 8]

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 9]

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 10]

**6.3 首页的实现**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 11]

图17 首页界面

**6.4 热门说说**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 12]

图18 热门说说

**6.5 用户之间的通讯**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 13]

图19 用户通讯

**6.6 用户个人中心设置**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 14]

图20 个人设置中心

**6.7 个人主页**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 15]

图21 个人界面

**6.8 我的说说，评论，收藏，点赞**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 16]

图22我的说说

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 17]

图23 我的评论

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 18]

图24 我的收藏

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 19]

图25 我的点赞

**6.9 说说评论**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 20]

图26 评论界面

**6.10 搜索**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 21]

图27 搜索

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 22]

图28 电影搜索结果

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 23]

图29 电影详情

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 24]

图30 音乐搜索

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 25]

图31 图书搜索

**6.11 音乐系统**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 26]

图32 热门音乐

**6.12 图书系统**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 27]

图33 图书推荐

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 28]

图34 图书详细信息

**6.13 查看正在上映的电影**

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 29]

图35 热映电影详情

![基于Spring Boot的“课程设计”的设计与实现][Spring Boot 30]

图36 热映电影评论

**7 备注**

下载地址：https://download.csdn.net/download/wsk1103/10484796

github地址：https://github.com/wsk1103/movie-boot

**首次启动项目**

1.  win系统安装Java 1.8 ， IDEA软件，MySQL数据库，redis，Nginx。
2.  打开MySQL，执行sql文件，将数据导入到MySQL中。
3.  将项目导入到IDEA中，构建为MAVEN项目。
4.  配置Nginx文件，使其负载均衡。
5.  待项目构建完成后，运行redis和Nginx（或者跳过Nginx）。
6.  修改resource文件中的application.properties，配置其中的数据库信息
7.  修改com.wsk.movie.email.Send文件中的用户账号和密码信息。
8.  由于使用了百度提供的图片识别功能，所以需要修改com.wsk.movie.tool.AuthService中百度提供的clientId和clientSecret（或者直接注释掉该类）
9.  将image.rar文件解压到D:/image，这个文件是存放图片和敏感词的重要文件。
10. 运行com.wsk.movie.MovieApplication的main方法。
11. 访问localhost


[Spring Boot]: /pro/os/crawler/Z7VU-IJUU-777Z.jpg
[Spring Boot 1]: /pro/os/crawler/QABM-RN3A-VZAE.jpg
[Spring Boot 2]: /pro/os/crawler/3M7B-2EYA-ZZUU.jpg
[Spring Boot 3]: /pro/os/crawler/UFAM-F2M6-77J3.jpg
[Spring Boot 4]: /pro/os/crawler/AAZJ-YBIZ-QM3E.jpg
[Spring Boot 5]: /pro/os/crawler/Z7BQ-A3AN-ZZN3.jpg
[Spring Boot 6]: /pro/os/crawler/NRQQ-NANZ-2IR2.jpg
[Spring Boot 7]: /pro/os/crawler/2EIN-JQ3A-EUE3.jpg
[Spring Boot 8]: /pro/os/crawler/JI73-2QF7-R7JV.jpg
[Spring Boot 9]: /pro/os/crawler/RBR7-V3JZ-MBFU.jpg
[Spring Boot 10]: /pro/os/crawler/MQJJ-7JQR-ZNEI.jpg
[Spring Boot 11]: /pro/os/crawler/VU7F-IZQZ-UVV2.jpg
[Spring Boot 12]: /pro/os/crawler/BQZB-7FQN-UFIZ.jpg
[Spring Boot 13]: /pro/os/crawler/RZI3-MZVF-BI3U.jpg
[Spring Boot 14]: /pro/os/crawler/EINY-VYQE-AFRA.jpg
[Spring Boot 15]: /pro/os/crawler/VAZV-3MBZ-3QVB.jpg
[Spring Boot 16]: /pro/os/crawler/BAME-V22A-EVVQ.jpg
[Spring Boot 17]: /pro/os/crawler/QEN6-NJIR-ZRNA.jpg
[Spring Boot 18]: /pro/os/crawler/3MZB-AMAF-ZABZ.jpg
[Spring Boot 19]: /pro/os/crawler/VZU2-AQFQ-V77N.jpg
[Spring Boot 20]: /pro/os/crawler/B7VJ-2AYR-EBEF.jpg
[Spring Boot 21]: /pro/os/crawler/ZVVE-JIAR-VEJB.jpg
[Spring Boot 22]: /pro/os/crawler/NBJ2-E2ME-BJAN.jpg
[Spring Boot 23]: /pro/os/crawler/FNNE-2YAA-MBIM.jpg
[Spring Boot 24]: /pro/os/crawler/AEFU-YZEJ-ABUR.jpg
[Spring Boot 25]: /pro/os/crawler/ZU2Q-ZJJV-Z3AA.jpg
[Spring Boot 26]: /pro/os/crawler/7VM2-2YNN-FUIR.jpg
[Spring Boot 27]: /pro/os/crawler/E3IN-3AAZ-NEVA.jpg
[Spring Boot 28]: /pro/os/crawler/UMAM-ZJMB-BRBJ.jpg
[Spring Boot 29]: /pro/os/crawler/UIMR-QNBE-F6RI.jpg
[Spring Boot 30]: /pro/os/crawler/MAJM-VR7V-UZVV.jpg
 *  **原文作者：** Java高级架构技术
 *  **原文链接：** https://www.toutiao.com/item/6568772862079926787/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。