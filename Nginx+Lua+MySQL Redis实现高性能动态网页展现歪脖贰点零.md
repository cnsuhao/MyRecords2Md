---
title: Nginx+Lua+MySQL Redis实现高性能动态网页展现
date: 2017-10-25 06:19:01
categories: "开发"
tags:
	- Nginx
	- NoSQL
	- MySQL
	- Lua
	- Redis

---

Nginx结合Lua脚本，直接绕过Tomcat应用服务器，连接MySQL/Redis直接获取数据，再结合Lua中Template组件，直接写入动态数据，渲染成页面，响应前端，一次请求响应过程结束。最终达到下图的一个效果。

![Nginx+Lua+MySQL/Redis实现高性能动态网页展现][Nginx_Lua_MySQL_Redis]

OpenResty组件

OpenResty的自带组件库默认已经集成了相当实用的组件，http://openresty.org/cn/components.html，如下所示：

 *  LuaCjsonLibrary
 *  LuaRestyMemcachedLibrary
 *  LuaRestyMySQLLibrary
 *  LuaRestyRedisLibrary
 *  LuaRestyWebSocketLibrary
 *  LuaRestyLimitTrafficLibrary
 *  其它等等

Lua-mysql 连接mysql

Lua直接连接MySQL的代码，再结全上一篇中连接Redis的代码，可以完成从后端动态的索取数据。

    --测试连接mysql，获取数据local function close_db(db) if not db then return end db:close()end//引入mysql组件local mysql = require("resty.mysql")local json = require("dkjson")local db, err = mysql:new()if not db then ngx.say("error : ", err) returnenddb:set_timeout(1000)local props = { host = "192.168.1.104", port = 3306, database = "sonar", user = "root", password = "root"}local res, err, errno, sqlstate = db:connect(props)if not res then ngx.say("connect error : ", err, " , errno : ", errno, " , sqlstate : ", sqlstate) return close_db(db)endlocal select_sql = "select * from dashboards"res, err, errno, sqlstate = db:query(select_sql)if not res then ngx.say("select error : ", err, " , errno : ", errno, " , sqlstate : ", sqlstate) return close_db(db)endfor i, row in ipairs(res) do for name, value in pairs(row) do ngx.say("select row =", i, " : ", name, " = ", value, "<br/>") end end close_db(db)

Lua-template 模板技术

通过Lua从后端动态取数，需要将数据渲染到静态页面，此时需要引入Template组件，该组件已经在OpenResty中引入，所以勿须再次安装，直接使用即可。

    --测试template组件，填充一些变量数据local template = require("resty.template") local context = {who = "guooo",from="usgrouping",jsons= {aaaa=123,bbbbb=23234}} //此处可调用mysql/redis，一同将数据写入template3.html文件中template.render("template3.html", context)

再看下静态页面模板长什么样：

    {(header.html)} <!doctype html><html lang="en"> <head> <meta charset="UTF-8"> <meta name="Generator" content="EditPlus®"> <meta name="Author" content="usgrouping"> <meta name="Keywords" content="usgrouping,guooo"> <title>lua-template test</title> </head> <body> <div>你好{{who}}，this is template page 3</div> <br/> <div>欢迎关注公众号：{{from}}</div> <br/> <br/> {{jsons}} </body></html>{(footer.html)} 其中header.html及footer.html是常用的头部和底部文件，这里只是简单的文本展示

经过上面的两大步，基本上就完成了动态数据经由Lua直接处理渲染成静态页面响应给前端，大大提高了执行效率。

> 有同学看了上一篇的例子，同时结合Lua连接mysql的例子发现，都是直接连接mysql/redis，而没有通过连接池的形式，其实完全可以使用连接池的形式，只不过此处为了说明原理，采用了直连的形式。

![Nginx+Lua+MySQL/Redis实现高性能动态网页展现][Nginx_Lua_MySQL_Redis 1]

 *  基于lua-nginx-module(openresty)的WEB应用防火墙
 *  Nginx+Lua+Redis实现高性能缓存数据读取
 *  介绍几款常用的在线API管理工具
 *  你不得不知的几个互联网ID生成器方案
 *  基于SpringCloud的Microservices架构实战案例-序篇
 *  目前在使用的Windows下最好用的shell
 *  如何编写无须人工干预的shell脚本


[Nginx_Lua_MySQL_Redis]: /pro/os/crawler/67R2-A3JR-IVZA.jpg
[Nginx_Lua_MySQL_Redis 1]: /pro/os/crawler/UYAQ-EIZ6-JBZM.gif
 *  **原文作者：** 歪脖贰点零
 *  **原文链接：** https://www.toutiao.com/item/6480605462579380750/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。