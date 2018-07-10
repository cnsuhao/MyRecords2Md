---
title: 为什么谷歌的JSON响应以while(1);开头？
date: 2014-01-11 12:39:39
categories: "开发"
tags:
	- JS

---

　**问题（QUESTION）：**

**　　**我有个问题一直很好奇就是：为什么谷歌的JSON响应以while(1);开头？举个例子，当把谷歌日历打开和关掉时，会返回这样的JSON对象：

``````````
while(1);[['u',[['smsSentFlag','false'],['hideInvitations','false'],['remindOnRespondedEventsOnly','true'],'hideInvitations_remindOnRespondedEventsOnly','false_true'],['Calendar ID stripped for privacy','false'],['smsVerifiedFlag','true']]]]
``````````

　　觉得这是谷歌不推荐使用eval()函数。我们需要做的是取代while，然后设置。我认为避免使用eval()函数是为了用eval()函数来确认JSON格式的字符串是否正确，从而确保程序员写出安全的JSON解析代码。我在其他一些地方也见过这个用法，但更多的是在谷歌的产品中，像谷歌的邮件、日历、通讯录等。

　　说来也奇怪，谷歌文档以&&&START&&&开头，但谷歌通讯录却以while(1);&&& START&&&开头。有人知道这是为什么吗？

　　**回答1：**

　　这可以防止JSON劫持。举个不是很恰当的例子：谷歌有一个URL：mail.google.com/json?Action=inbox ，在JSON格式下，它可以读取你的收件箱内前50条消息。由于同源策略，其他域内的坏网站却不能发出AJAX请求，以获取这些数据，但是他们可以通过重写全局数组构造函数或者存取函数，使cookies和URL间可以有一种方法，无论何时设置一个对象（数组或散列）的属性，都允许他们读取JSON内容。

　　while(1)； 或者&&&BLAH&&& 可以防止mail.google.com上的一条AJAX请求，拥有对于文本内容的全部访问权限，并且能够将其撤销。但是同时，一 个 SCRIPT 标签的插入会在无需任何处理的情况下，盲目地执行JavaScript，最终可能导致死循环或语法错误。同时这也并不能解 决伪造的跨站请求问题。

**　　回答2：**

**　　**这是为了确保让其他网站不能偷取你的信息。例如，先 通过替换数组构造器，再通过一个 SCRIPT 标签包含这个JSON URL，恶意的第三方网站就可以从JSON响应盗取数据。把while(1);放在开头就可以解决这样的问题。在另一方面，来自同一个网站使用XHR和独 立的JSON解析器的请求，可以直接忽略while(1);。

**　　回答3：**

**　　**这将会给使用 SCRIPT 标签的第三方网站在HTML文档中插入JSON响应变得困难。值得注意的是， SCRIPT 标签
是不需要被Sa授权的哦。

**　　回答4：**

**　　**这可以防止被当作一个简单 SCRIPT 标签目标。这样的话，坏家伙就不能轻而易举地把脚本标签放到他们自己的网站，利用活动会话而有机会偷取你的数据。

\--原文:[http://stackoverflow.com/questions/2669690/why-does-google-prepend-while1-to-their-json-responses?utm\_source=ourjs.com][http_stackoverflow.com_questions_2669690_why-does-google-prepend-while1-to-their-json-responses_utm_source_ourjs.com]

\--转自:http://www.admin10000.com/document/3693.html


[http_stackoverflow.com_questions_2669690_why-does-google-prepend-while1-to-their-json-responses_utm_source_ourjs.com]: http://stackoverflow.com/questions/2669690/why-does-google-prepend-while1-to-their-json-responses?utm_source=ourjs.com