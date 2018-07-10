---
title: .valueof()的简化用法+
date: 2013-02-04 10:29:11
categories: "开发"
tags:
	- 技术
	- javascript

---

在查看Ext.Date扩展方法时看到now的函数代码中：now:Date.now||function()\{

return +new Date();

\}

查询以下解释： 做下测试即可。

var s=+newDate();

解释如下:=+是不存在的;

\+new Date()是一个东西;

\+相当于.valueOf();

``````````
//三个结果一样返回当前时间的毫秒数
alert(+new Date());
alert(+new Date);
var s=new Date();
alert(s.valueOf());
``````````

顺便说下valueOf的另一个用法:

valueOf() 方法可返回 Boolean 对象的原始值。

如果调用该方法的对象不是 Boolean，则抛出异常 TypeError。

``````````
var boo = new Boolean(false);
document.write(boo.valueOf());
``````````