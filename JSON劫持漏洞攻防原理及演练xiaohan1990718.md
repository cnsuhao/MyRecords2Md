---
title: JSON劫持漏洞攻防原理及演练
date: 2014-01-11 12:32:52
categories: "开发"
tags:
	- JS

---

**注\* 作者发表这篇文章的时间较早，某些方法可能并不是最好的解决方案，但针对这种漏洞进行的攻击还依然可见，如早期的：[QQMail邮件泄露漏洞][QQMail]，下面介绍的是对这种攻击原理的介绍。**

　　不久之前，我写了一篇文章《一个微妙的JSON漏洞》，文中讲到这个漏洞可能会导致敏感信息泄露。针对该漏洞的特点，通过覆盖JavaScript数组构造函数以窃取(暴露)JSON返回数组，而现在大多数浏览器还无法防范这种攻击。

　　然而，通过和微软的Scott Hanselman交流，我了解到另外一个方法可能会影响更多的浏览器。在上周的挪威开发者大会上，我做了一个针对Json劫持漏洞的演示。

　　在我进一步讲之前，我先说一说，这个漏洞可能带来的影响。

　　在以下条件下，会出现这个漏洞：首先暴露JSON服务，并且该服务会返回敏感数据；返回JSON数组；对GET请求做出响应；发送这个请求的浏览器启用了JavaScript并且支持\_defineSetter\_方法。

　　如果我们不使用JSON发送敏感数据，或者只对报文请求做出响应，那么我们的网站就不存在这个漏洞。

　　我不喜欢用流程图展示这个过程，我会尽量用图表描述。在第一页截图上，我们可以看到不知情的受害者登陆漏洞网站，漏洞网站返回了一个身份认证的cookie。

![Z7NI-22JF-3EJF.jpg][]


我们都可能收到过一些垃圾邮件，邮件中附有链接，发送者声称这有一段搞笑视频。这些大量的垃圾邮件都是一些别有用心的人发的。


![6VZV-A32M-QEAU.jpg][]


但是实际上，这些链接指向的是那些坏家伙自己网站。当我们点击了链接，接下来的两个步骤会迅速进行。第一，我们的浏览器向这些网站发送请求。


![MEEJ-7NRF-UFMR.jpg][]


第二，那些网站会响应一些包含JavaScript的HTML。这些JavaScript会带一个script标记。当浏览器检测到script标记，它就会向那些漏洞网站再发一个下载脚本的GET请求，携带着身份验证的cookie。


![Z7RE-U2RV-2QUM.jpg][]


这样，那些坏家伙就伪装成了受害者的浏览器，利用其身份发出了一个包含敏感数据的JSON请求。接着，把JSON加载为可执行的JavaScript，这样以来，那些黑客就能够获取到这些数据。
为了加深理解，我们可以看看一个攻击的实际代码。假如漏洞网站返回带有敏感数据的JSON响应通过如下方式发送：

``````````
[Authorize]
publicJsonResultAdminBalances(){
    varbalances=new[]{
        new{Id=1,Balance=3.14},
        new{Id=2,Balance=2.72},
        new{Id=3,Balance=1.62}
    };
    returnJson(balances);
}
``````````

　　需要说明的是，上面的演示不是专门针对ASP.NET或者ASP.NET MVC，我仅仅是恰巧用ASP.NET MVC来演示这个漏洞而已。
假如这是HomeController的一种方法，我们通过对/Home/AdminBalances发送了一个GET请求，并且返回如下JSON文本：

``````````
[{“Id”:1,”Balance”:3.14},{“Id”:2,”Balance”:2.72},{“Id”:3,”Balance”:1.62}]
``````````

　　注意，我定义这个方法时使用了Authorize属性，用来验证请求者的身份。所以一个匿名的GET请求将不会得到敏感数据。
重要的是：这是一个JSON数组。包含JSON数组的文本是一个有效的JavaScript脚本，并且可以被执行。仅仅包含JSON对象的脚本不是一个有效的JavaScript可执行文件。
举个例子，如果我们有一个包含如下JSON代码的JavaScript文档：

``````````
{“Id”:1,”Balance”:3.14}
``````````

　　并且有一个指向这个文档的脚本标签：

``````````
<script src=”http://example.com/SomeJson”></script>
``````````

　　这样，我们会在HTML页中得到一个JavaScript错误。然而，倘若存在一个不幸的巧合，如果我们有一个script标签，这个标签指向仅仅含有一个JSON数组的文档，这样的话，这个标签就会被误认为是有效的JavaScript，并且数组会生效。

　　下面就让我们看看那些别有用心的人的服务器上的HTML页。

　　注\*这里我们可以看到使用 Json Object 而不是Json Array返回你的数据，可以在一定程度上预防这种漏洞。

``````````
<html>
...
<body>
<scripttype="text/javascript">
Object.prototype.__defineSetter__('Id',function(obj){alert(obj);});
</script>
<scriptsrc="http://example.com/Home/AdminBalances"></script>
</body>
</html>
``````````

　　看到了什么？黑客正在改变对象的原型，用\_defineSetter\_这种特殊方法，覆盖JSON对象（Object）原本应有的默认行为。

　　在这个例子中，一个命名合适的ID在任何时候都可以被设置到任何对象上时，一个匿名的函数将会被调用，这个函数将会利用alert 函数显示属性值。注意，这时脚本仅仅会将数据发回给那些坏家伙，而不会发送敏感数据。

　　就像之前提到的，坏家伙需要使我们在登录漏洞网站后不久并且在会话仍旧有效时，访问他的恶意的网页。通过含有恶意网站链接的邮件进行钓鱼攻击的方式是很典型的。

　　如果你仍旧点击链接登录原网站，浏览器将会在加载脚本标签中引用的脚本并向网站发送你的身份验证cookie。直到连接上原网站，我们对JSON数据发出一 个有效的身份验证请求，并且会收到在我们浏览器中响应的有效数据。这些话可能听着很熟悉，因为它是一个真的变种伪造跨站请求，之前我写过这种情况。

　　因为在IE8上\_defineSetter\_是一个无效的方法，所以在IE8上看不到现象。我在Chrome和Firefox上都试过，都可以。

　　避免这个漏洞也很简单：或者从不发送JSON数组，或者只访问HTTP POST以获得需要的数据。举个例子：在ASP.NET MVC中，你可以用AcceptVerbsAttribute来实现：

``````````
[Authorize]
[AcceptVerbs(HttpVerbs.Post)]
publicJsonResultAdminBalances(){
    varbalances=new[]{
        new{Id=1,Balance=3.14},
        new{Id=2,Balance=2.72},
        new{Id=3,Balance=1.62}
    };
    returnJson(balances);
}
``````````

　　这个方法的一个问题就是：像jQuery这样的很多JS库默认都是用GET方式发送JSON请求，而不是POST。举个例子，$.getJSON 默认发起的是GET请求。所以，当进行这种JSON访问时，我们需要确信我们是用客户端库发起的POST请求。

　　ASP.NET和WCF JSON服务端实际上在对象中用了“d”属性，包裹了他们的JSON，这我在另一篇[文章][Link 1]中讨论过：

　　必须经过这些属性获得数据，似乎有些奇怪，但这需要通过一个客户端代理来实现来去除“d”属性，，以便不影响最终用户。

　　在ASP.NET MVC下，绝大多数开发者没有生成客户端代理，而是使用jQuery和其他类似的库，这样一来使用“d”属性的就有些尴尬。

**　　注\*其实MVC的方法有点复杂，到这里我们可以看出，JSON劫持漏洞是要以在受豁者的浏览器上执行JSON返回对象为前提的，其实Google使用了一种更加聪明的方法，通过添加“死循环”命令，防止黑客运行这段脚本，可参见这篇文章：[为什么谷歌的JSON响应以while(1);开头？ ][JSON_while_1_]**

**　　检查首部（Http-Header）怎么样？**

**　　**一些人可能会有疑问：“为什么不在响应一个GET请求之前，用一个特殊的首部对JSON服务进行检查？就像X-Requested- With:XMLHttpRequest或者Content-Type:application/json”。我认为这可能是个过渡，因为大多数的客户端 库会发送一种或两种Header，但是浏览器响应脚本标签的GET请求不这样。

　　问题是：在过去某个时候，用户可能会发出合法的JSON GET请求，在这种情况下，这个漏洞可能会隐藏在用户浏览器的正常请求之间。也是在此种情况下，当浏览器发出GET请求，这种请求可能会缓存在浏览器和代 理服务器的缓冲区。我们可以尝试着设置No-Cache header，这样，我们信任浏览器和所有的代理服务器能够正确地实现高速缓存并且信任用户也不会被意外地覆盖。

　　当然，如果我们用SSL提供JSON文本，这个特定的缓存问题将会很容易被解决。

　　注\*这里我们可以看到防范方法这三：不要cache你的ajax请求，不过目前似乎所有的js库默认都是不cache的。

**　　真正的问题在哪里？**

**　　**Mozilla Developer Center发表的一篇文章中写道：对象和数组初始化设定项在赋值时不应该调用setters方法。这一点我同意。尽管有评论认为：也许浏览器真的不应该执行脚本。

　　但是在一天结束的时候，分派责任并不能使你的网站更加安全。这些浏览器的怪癖毛病将会时不时地出现。我们作为网站的开发者需要解决这些问题。 Chrome2.0.172.31和Firefox3.0.11也有这个软肋。IE8没有这个问题，因为它不支持这种方法，我也没有在IE7或者IE6中 试验过。

　　在我看来，在当前的客户端库下，安全访问JSON的默认方式应该是POST，并且我们应该选择GET,而不是其他方式。您觉得呢？您所了解的其他平台是怎么解决这个问题的呢？我很想听听大家的想法。


\--原文:[http://haacked.com/archive/2009/06/25/json-hijacking.aspx/?utm\_source=ourjs.com][http_haacked.com_archive_2009_06_25_json-hijacking.aspx_utm_source_ourjs.com]

\--转自:http://www.admin10000.com/document/3694.html




[QQMail]: http://www.wooyun.org/bugs/wooyun-2010-046
[Z7NI-22JF-3EJF.jpg]: /pro/os/crawler/Z7NI-22JF-3EJF.jpg
[6VZV-A32M-QEAU.jpg]: /pro/os/crawler/6VZV-A32M-QEAU.jpg
[MEEJ-7NRF-UFMR.jpg]: /pro/os/crawler/MEEJ-7NRF-UFMR.jpg
[Z7RE-U2RV-2QUM.jpg]: /pro/os/crawler/Z7RE-U2RV-2QUM.jpg
[Link 1]: http://haacked.com/archive/2008/11/20/anatomy-of-a-subtle-json-vulnerability.aspx/
[JSON_while_1_]: http://www.admin10000.com/document/3693.html
[http_haacked.com_archive_2009_06_25_json-hijacking.aspx_utm_source_ourjs.com]: http://haacked.com/archive/2009/06/25/json-hijacking.aspx/?utm_source=ourjs.com