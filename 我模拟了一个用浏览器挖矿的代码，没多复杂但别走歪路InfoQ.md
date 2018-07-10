---
title: 我模拟了一个用浏览器挖矿的代码，没多复杂但别走歪路
date: 2018-03-12 10:44:26
categories: "开发"
tags:
	- 脚本语言
	- 软件
	- JavaScript
	- 编程语言
	- WebApp

---

点击关注 **InfoQ**，置顶公众号

接收程序员的 8 点技术早餐

![我模拟了一个用浏览器挖矿的代码，没多复杂但别走歪路][YMUR-MFNU-FZ6J.jpg]

作者｜Ben

编辑｜Martin、郭蕾

出处丨聊聊架构

最近比特币大火，区块链大火，昨天就连我一个四线城市的、没怎么接触过互联网的小学同学都焦虑地打电话问我，区块链这波浪潮里他应该怎么挣钱。

聊聊架构之前发布过一系列的文章来介绍区块链技术，想想真有意思，那会阅读量怎么都做不高，但现在，又突然出来很多的区块链媒体。当然，我们也做了一个专注报道区块链技术的公众号：区块链前哨，欢迎大家关注。

之前，我估计很多人都注意到过这样一个新闻，有一些成人网站看到比特币的暴涨之后，开始打起了挖矿的主意。他们在自己网站中偷偷暗藏挖矿代码，而当用户访问这些网站之后，他们就会神不知鬼不觉地迅速运行这些挖矿代码，甚至在用户关闭浏览器之后，相关代码仍然能够继续运行并占用 CPU 资源。

今天这篇是篇翻译文章，作者 Ben 是一名国外的资深开发者。他看到这篇新闻之后，也陷入了沉思。换个角度，**如果不是挖矿，而是把全世界的 Web 浏览器通过 websocket 连接在一起，那是不是就可以组成一台“超级计算机”，并利用这个超级计算机解决分布式问题？**

于是他开始了新的尝试。聊聊架构对这篇文章进行了翻译，以下是全文。

写在前面

我们将讨论一个具有争议性的话题——如何从网站访客的浏览器中“偷”走计算资源。目前有很多讨论是关于如何利用浏览器来挖掘数字货币的，但我不想加入到这些话题的讨论当中，我只是想探讨一种有效利用计算资源的方式。

Web 浏览器执行代码的能力越来越强大。JavaScript 的发展、WebAssembly 的出现、对 GPU 访问能力的提升和线程模型的演变，这些因素组合在一起，让浏览器与计算机一样具备了强大的计算能力。随着浏览器数字货币挖矿机的崛起，我也在思考这样的一个问题：如何把全世界的计算资源整合成一个单独的实体——一台由网站访客的浏览器组成的超级计算机。

就像普通的计算机集群一样，这台超级计算机的所有计算节点在协调之下共同解决一个问题。但与普通的计算机集群不同的是，这些计算节点时临时性的（随着网站访客的来来去去），而且它们之前无法彼此对话（没有跨站点的请求）。

这是我想到的一个例子：

![我模拟了一个用浏览器挖矿的代码，没多复杂但别走歪路][EYN2-AEZJ-RJUB.jpg]

右边是超级计算机控制服务器。左边是访问某个网站的浏览器，它是这个超级计算机中的一个节点，上面还显示了它的 CPU 指标。

这个超级计算机要解决的问题是找出某个给定哈希值的原始值。从图上可以看出，总共有 23 个节点参与了计算，计算并比对了 380,204,032 个哈希值，其中美国的访客贡献了 50% 的处理能力。

代码实现

这里主要用 websocket 技术在服务器和计算节点之间建立持久连接。这些连接用于协调节点的行为，从而让它们成为相互协作的实体。websocket 可以传输代码和协作消息，让一切都成为可能。

websocket 的出现戏剧性地改变了 Web 客户端的行为。客户端连接到网站上，先执行预先定义好的 JavaScript，等建立起 websocket 连接之后，就可以执行其他任意 JavaScript 脚本。

下图右边是超级计算机控制服务器，左边是接收动态指令的 Web 客户端。

![我模拟了一个用浏览器挖矿的代码，没多复杂但别走歪路][ENM6-JU2E-6BY3.gif]

如果一款 App 使用了 WebView，JavaScript 就可以直接跑到 App 中，也就是说，通过 websocket 传输的代码可以跳过 WebView，直接进入 App 的领地。

下图右边是超级计算机控制服务器，左边是接收指令的 Web App。可以看到，指令直接渗透到了 App 层。

![我模拟了一个用浏览器挖矿的代码，没多复杂但别走歪路][NIVA-3IUR-YZBJ.gif]

剩下的就没有什么新鲜的了。App 可以通过 C&C 协议（Botnet Command and Control）获取指令，网页在初次加载之后就可以动态获得 JavaScript 脚本，而 websocket 具有真正的动态性（不像 Ajax 的轮询拉取模式），可以跨多浏览器和设备运行，而且对运行环境有完全的访问权限。

所以，我们完全可以通过 websocket 向计算节点传输指令代码，当然也可以用来传递消息，实现分布式协调。

Crackzor.js

六年前，我基于 OpenMPI 开发了一款分布式密码破解器（ben.akrin.com/?p=1424），叫作 crackzor。

密码破解是一个非常典型的分布式问题，说起来很简单，就是通过排列组合字符猜出密码。我使用 JavaScript 重写了 crackzor，使用 websocket 替代了 OpenMPI。

不过，每一个分布式问题都是不一样的，crackzor 并不是解决所有问题的良方。crackzor 的魔力在于它的灵活性，它把一个字符排列组合空间拆分成很多个块，再把这些块分摊给计算节点。在给定了要解决的问题以及迭代的起始和结束位置之后，节点就可以开始工作，不需要再为它们提供字符排列组合，这样就不会出现网络带宽瓶颈问题。

第一个问题：如何最大程度利用节点的 CPU

JavaScript 默认使用的是单线程模型，代码通过 websocket 传送到客户端，默认情况下只使用了 CPU 的一个核。而现今的大部分计算机 CPU 都是多核的，所以，我们要想办法把这些 CPU 都利用起来。

于是救星出现了——Web Worker。HTML 5 提供了这一特性，极大简化了多线程的实现。不过，我们还需要解决一个问题。Web Worker 文档告诉我们要从文件加载脚本文件，但我们的代码是通过 websocket 传输过来的，并驻存在内存中，所以我们无法直接通过指定脚本文件的方式来执行代码。

我们通过把代码包装成一个 Blob 对象来解决这个问题：

``````````
var worker_code = 'alert( "this code is threaded on the nodes" );' window.URL = window.URL || window.webkitURL; var blob; try { blob = new Blob([worker_code], {type: 'application/javascript'}); } catch (e) { window.BlobBuilder = window.BlobBuilder || window.WebKitBlobBuilder || window.MozBlobBuilder; blob = new BlobBuilder; blob.append(worker_code); blob = blob.getBlob; } workers.push( new Worker(URL.createObjectURL(blob)) ) ;
``````````

第二个问题：在节点间分配任务

Websocket 服务器承担了后续的大部分协调工作，它需要跟踪节点的接入和退出，以及某个节点是否在执行计算任务，并在有可用节点时给它们分配任务。

服务器需要一直处于运行状态，处理来自节点的连接。不过，这台超级计算机可能每天需要解决不同的问题。为此，我写了一个函数用来读取文件，并执行文件中的代码。这个函数通过一个进程来调用。

``````````
function eval_code_from_file { if( !file_exists("/tmp/code") ) { console.log( "error: file /tmp/code does not exist" ) ; } else { var code = read_file( "/tmp/code" ) ; code = code.toString ; eval( code ) ; } } process.on('SIGUSR1', eval_code_from_file.bind );
``````````

有了这个函数，下一次我就可以杀掉旧进程，然后使用新进程加载新代码。这要归功于 JavaScript 的灵活性，这种灵活性让我们可以在任意时刻运行任意代码，只要对运行环境有完全的访问权限。

要给节点分发任务也很简单，只要让客户端在连接到服务器时注册一个回调函数，然后在回调函数里执行代码即可。

比如客户端：

``````````
var websocket_client=io.connect("http://websocket_server.domain.com"); websocket_client.on( "eval_callback",function(data){data=atob(data),eval(data)}.bind ) ;
``````````

服务器端：

``````````
client_socket.emit( "eval_callback", new Buffer("alert('this code will run on the client');").toSt
``````````

到目前为止：

1.  所有的临时节点（网站用户的 Web 浏览器）连接到 websocket 服务器上；
2.  通过进程信号让 websocket 服务器执行新的代码；
3.  新代码中包含了节点需要解决的新问题；
4.  新代码告诉 websocket 服务器如何协调节点；
5.  一旦某个节点解决了问题，接着处理下一个问题；

现在我们知道了如何利用 Web 浏览器来构建一台超级计算机。出于多方面的考虑，比如可读性、安全性和复杂性方面的问题，我不想把我的代码全部都公开出来。不过，如果有人感兴趣，可以联系我，我很乐意分享跟多的想法。

更多小建议

 *  在拆分任务时，任务不能太大。因为节点都是临时性的，如果任务太重，极有可能发生中断。大部分 Web 浏览器会拒绝执行或终止执行太耗资源的代码，而小任务可以在几秒钟之内就完成，不会被打断。
 *  使用 JavaScript 实现 MD5：https://gist.github.com/josedaniel/951664。
 *  记录节点解决问题所使用的平均时间，把运行缓慢的节点排除在外，以免影响“超级计算机”的整体性能。


[YMUR-MFNU-FZ6J.jpg]: /pro/os/crawler/YMUR-MFNU-FZ6J.jpg
[EYN2-AEZJ-RJUB.jpg]: /pro/os/crawler/EYN2-AEZJ-RJUB.jpg
[ENM6-JU2E-6BY3.gif]: /pro/os/crawler/ENM6-JU2E-6BY3.gif
[NIVA-3IUR-YZBJ.gif]: /pro/os/crawler/NIVA-3IUR-YZBJ.gif
 *  **原文作者：** InfoQ
 *  **原文链接：** https://www.toutiao.com/item/6531883615138087431/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。