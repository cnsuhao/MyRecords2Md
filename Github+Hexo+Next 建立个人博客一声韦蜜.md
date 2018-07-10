---
title: Github+Hexo+Next 建立个人博客
date: 2017-10-17 19:21:11
categories: "开发"
tags:
	- GitHub
	- Hexo
	- Windows 7
	- Node.js
	- Bash

---

> 如需转载，请联系本人，感谢合作。

![Github+Hexo+Next 建立个人博客][Github_Hexo_Next]

# 导语 #

这篇博文主要介绍如何利用 Hexo 博客框架，加上 Next 主题，将博客内容托管在 Github 上 。最终您可以通过访问如 「zhangwade.github.io」 的地址格式，访问到自己的个人博客网站。网上这种教程有一大堆，但有一些年代较久远而且感觉都有点小瑕疵，我就把自己的整个流程记录下来，供大家参考，也方便自己回顾。

友情提醒：教程只能告诉你大概步骤，想少入一点坑还是多看官方操作文档，准没错。

# 一. 所需软件工具 #

\* \[Node.js\](https://nodejs.org/zh-cn/) \*\*v6.10.0\*\*

\* \[Hexo\](https://hexo.io/zh-cn/) \*\*v3.2.2\*\*

\* \[Next\](http://theme-next.iissnan.com/)

\* \[Github\](https://github.com/)

\* \[多说评论系统\](http://duoshuo.com/)

PS: 上述工具后面 v 开头的字符串是指我用的版本号，另外，我的电脑系统是 Win7 旗舰版。本博文不介绍如何创建 Github 帐号，下载、使用 Git 工具以及 SSH，这是程序猿必备的东西，自行科学上网了解。

# 二. 安装 Node.js 和 Hexo #

简介： Hexo 是一个用 Node.js 写的博客框架。

 *  点击上面的链接下载安装 Node.js
 *  随意在桌面右键点击打开 Git Bash，输入以下命令安装 Hexo。

\`npm install -g hexo-cli\`

 *  在某个盘下新建文件夹，例如 「D:Hexo」，然后对着该文件夹右键点击 Git Bash，输入以下命令创建所需的文件。

\`hexo init\`

 *  继续输入以下命令安装所需依赖包。

\`npm install\`

 *  继续输入以下命令，完成在本地的网页部署。

\`hexo g\`

\`hexo s\`

 *  此时，浏览器输入 \[http://localhost:4000\](http://localhost:4000) ，正常可以看到 Hexo 默认的博客框架样式。\*\*如若无法打开网页，是默认的 4000 端口号被占用了，此时就要执行以下命令把端口号改成其它未被占用的端口\*\*。

\`hexo server -p 5000\`

 *  查看端口是否被占用，可以在 cmd 命令行输入以下命令。

\`netstat –ano|findstr 4000\`

# 三. 在 Github 创建个人博客网站专属的 Repository 并把本地 Hexo 博客文件部署到该 Repository 上 #

 *  如何新建一个 Repository 这里就不细说了，主要注意 Repository 名字要遵照以下规则，前面必须与你的 Github 名字一致。

\`zhangwade.github.io\`

「zhangwade」 此处改为你自己的 Github 名字。最终效果如下：

![Github+Hexo+Next 建立个人博客][Github_Hexo_Next 1]

 *  找到之前安装 Hexo 的文件夹，打开里面的 「\_config.yml」 文件，编辑。

\`deploy:\`

　\`type: git\`

　\`repo: git@github.com:zhangwade/zhangwade.github.io.git\`

　\`branch: master\`

这里的 repo 我是用的 SSH 地址，网上其它教程有的填的是 HTTPS 地址，反正我这里用的 SSH 是成功的。关于如何配置 SSH 密钥使本地仓库与 Github 仓库相关联，本文也不细说。另外 type 填 git 而不是以前的 github 也没有疑问。注意冒号「：」与后面的文字之间是有一个空格的。

 *  同样对着 Hexo 文件夹右键，点击 Git Bash，输入以下命令，部署文件到 Github 上。

\`hexo g\`

\`hexo d\`

此时若显示 ERROR 信息，即需要安装如下模块，输入以下命令安装：

\`npm install hexo-deployer-git --save\`

此时若再显示其它的 ERROR 信息，执行以下命令：

\`git config --global http.sslverify false\`

以上命令参考 \[SSL certificate problem\](https://github.com/composer/composer/issues/3908)

另外还遇到过一个 ERROR ，具体是哪一步出现的就忘了，还是解决的链接贴上来

\[ERROR解决办法\](http://stackoverflow.com/questions/20747817/error-unable-to-verify-leaf-signature-phonegap-installation)

解决以上 ERROR 后，再执行 「hexo d」 部署以下，登录你的 io 结尾的网址，如

「zhangwade.github.io」，此时便大功告成了！

# 四. 更换 Next 主题 #

 *  具体参考官方文档 \[开始使用- NexT 使用文档\](http://theme-next.iissnan.com/getting-started.html) ，因为真的写得非常清晰了。

# 五. 添加 「多说」 评论模块 #

 *  同样具体参考文档 \[多说评论\](http://theme-next.iissnan.com/getting-started.html\#comment-system-duoshuo) ，Next 主题本来就集成了 「多说」 模块，只要在 「多说」 注册帐号，然后在 Next 配置文件中绑定相应的帐号，重新再部署一下就可以了。


[Github_Hexo_Next]: /pro/os/crawler/YJB2-U3FU-YUZ2.jpg
[Github_Hexo_Next 1]: /pro/os/crawler/INNV-YVJ2-I32Q.jpg
 *  **原文作者：** 一声韦蜜
 *  **原文链接：** https://www.toutiao.com/item/6477838344947515917/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。