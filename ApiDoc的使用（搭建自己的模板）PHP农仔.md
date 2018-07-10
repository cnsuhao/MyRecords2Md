---
title: ApiDoc的使用（搭建自己的模板）
date: 2018-04-16 08:05:14
categories: "开发"
tags:
	- 技术
	- JSON
	- BASIC语言
	- Markdown

---

命令行

安装ApiDoc后，查看命令行参数

**$apidoc -h**

如下：

![ApiDoc的使用（搭建自己的模板）][ApiDoc]

**命令行使用**

\-h 显示帮助信息

\-i input（读取），要生成文档的代码目录

\-o 生成api文档静态页面的目录

\-t 自定义的模板目录，默认使用apiDoc的模板

会使用这几个命令足够，其它没必须，用为工具，够用即可。

apidoc生成文档实战

 **1、先配置apidoc.json**

在项目的根目录下新建一个Json文件，命名apidoc.json。

apidoc.json的内容如下：

\{

“name”: “Test API Document”,

“version”: “0.1.0”,

“description”: “用于Apidoc教程的的文档”,

“title”: “Test API”,

“url” : “www.lianganhui.cn”,

“sampleUrl”: “http://www.lianganhui.cn/test”,

\}

name

文档内容的最大标题

version

文档的版本号，一般保持在最新

description

文档的描述

title

显示网页的title

url

每个api地址前缀

sampleUrl

请求示例工具的地址前缀，当有此项时，会出现该工具

header/footer 文档的头部和尾部

 *  title 头/尾部标题

 *  filename 头部markdown文件

template

 *  withCompare 自动生成版本比较功能的文件，默认 true

 *  withGenerator 生成默认的apidoc版权，默认 true

2、接口注释（案列是拿官网案例）

在项目根目录下新建文件夹test，在test文件夹新建example.js。

``````````
/** Basic Example** This is a basic example for apiDoc.* Documentation blocks without @api (like this block) will be ignored.*//*** @api {get} /user/:id Get User information* @apiName GetUser* @apiGroup User** @apiParam {Number} id Users unique ID.** @apiSuccess {String} firstname Firstname of the User.* @apiSuccess {String} lastname Lastname of the User.** @apiSuccessExample Success-Response:* HTTP/1.1 200 OK* {* "firstname": "John",* "lastname": "Doe"* }** @apiError UserNotFound The <code>id</code> of the User was not found.** @apiErrorExample Error-Response:* HTTP/1.1 404 Not Found* {* "error": "UserNotFound"* }*/
``````````

3、生成文档的命令

$ apidoc -i test/ -o doc/

生成成功后会看到一个新的文件夹doc：

打开文件夹doc，点击index.html

![ApiDoc的使用（搭建自己的模板）][ApiDoc 1]

可看到APIDoc对中文会出现乱码。（建议创建文件时使用UTF-8编码）

到目前已经搭建完成。

4、创建自己的文档模版（我是一个有个性的人）

（1）、下载模板：https://github.com/apidoc/apidoc/tree/master/template

（2）、下载下来后，根据自己喜爱，调整样式。

（3）、调整后放到项目目录下。

![ApiDoc的使用（搭建自己的模板）][ApiDoc 2]

（4）、完美生成自己的个性模板。

$ apidoc -i test/ -o doc/ -t template\_test

修改

\{

“name”: “Test API Document”,

“version”: “0.1.0”,

“description”: “用于Apidoc教程的的文档”,

“title”: “Test API”,

“url” : “www.lianganhui.cn”,

“sampleUrl”: “http://www.lianganhui.cn/test”,

“header”: \{

“title”: “Overview”,

“filename”: “template\_test/header.md”

\},

“footer”: \{

“title”: “Copyright”,

“filename”: “template\_test/footer.md”

\},

“template”: \{

“forceLanguage”:”zh\_cn”，

“withGenerator”: false

\}

\}

效果：

![ApiDoc的使用（搭建自己的模板）][ApiDoc 3]


[ApiDoc]: http://p1.pstatp.com/large/pgc-image/152383708937193790e3579
[ApiDoc 1]: http://p1.pstatp.com/large/pgc-image/15238370896374860ee8eae
[ApiDoc 2]: /pro/os/crawler/EFUN-VFB6-R2Q2.jpg
[ApiDoc 3]: http://p3.pstatp.com/large/pgc-image/1523837089587448f4e2de6
 *  **原文作者：** PHP农仔
 *  **原文链接：** https://www.toutiao.com/item/6544830572785566211/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。