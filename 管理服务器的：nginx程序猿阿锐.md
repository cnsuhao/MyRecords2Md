---
title: 管理服务器的nginx
date: 2018-01-29 10:40:08
categories: "开发"
tags:
	- Nginx
	- BSD
	- Linux
	- 软件
	- Apache

---

![管理服务器的：nginx][nginx]

# 什么是 Nginx  #

Nginx 是俄罗斯人编写的十分轻量级的 HTTP 服务器,Nginx，它的发音为“engine X”，是一个高性能的HTTP和反向代理服务器，同时也是一个 IMAP/POP3/SMTP 代理服务器。Nginx 是由俄罗斯人 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发��，它已经在该站点运行超过两年半了。Igor Sysoev 在建立的项目时,使用基于 BSD 许可。

英文主页：http://nginx.net 。

到 2013 年，目前有很多国内网站采用 Nginx 作为 Web 服务器，如国内知名的新浪、163、腾讯、Discuz、豆瓣等。据 netcraft 统计，Nginx 排名第 3，约占 15% 的份额(参见：http://news.netcraft.com/archives/category/web-server-survey/ )

Nginx 以事件驱动的方式编写，所以有非常好的性能，��时也是一个非常高效的反向代理、负载平衡。其拥有匹配 Lighttpd 的性能，同时还没有 Lighttpd 的内存泄漏问题，而且 Lighttpd 的 mod\_proxy 也有一些问题并且很久没有更新。

现在，Igor 将源代码以类 BSD 许可证的形式发布。Nginx 因为它的稳定性、丰富的模块库、灵活的配置和低系统资源的消耗而闻名．业界一致认为它是 Apache2.2＋mod\_proxy\_balancer 的轻量级代替者，不仅是因为响应静态页面的速度非常快，而且它的模块数量达到 Apache 的近 2/3。对 proxy 和 rewrite 模块的支持很彻底，还支持 mod\_fcgi、ssl、vhosts ，适合用来做 mongrel clusters 的前端 HTTP 响应。

# Nginx 特点 #

Nginx 做为 HTTP 服务器，有以下几项基本特性：

 *  处理静态文件，索引文件以及自动索引；打开文件描述符缓冲．
 *  无缓存的反向代理加速，简单的负载均衡和容错．
 *  FastCGI，简单的负载均���和容错．
 *  模块化的结构。包括 gzipping, byte ranges, chunked responses,以及 SSI-filter 等 filter。如果由 FastCGI 或其它代理服务器处理单页中存在的多个 SSI，则这项处理可以并行运行，而不需要相互等待。
 *  支持 SSL 和 TLSSNI．

Nginx 专为性能优化而开发，性能是其最重要的考量,实现上非常注重效率 。它支持内核 Poll 模型，能经受高负载的考验,有报告表明能支持高达 50,000 个并发连��数。

Nginx 具有很高的稳定性。其它 HTTP 服务器，当遇到访问的峰值，或者有人恶意发起慢速连接时，也很可能会导致服务器物理内存耗尽频繁交换，失去响应，只能重启服务器。例如当前 apache 一旦上到 200 个以上进程，web响应速度就明显非常缓慢了。而 Nginx 采取了分阶段资源分配技术，使得它的 CPU 与内存占用率非常低。Nginx 官方表示保持 10,000 个没有活动的连接，它只占 2.5M 内存，所以类似 DOS 这样的攻击对 Nginx 来说基本上是毫无用处的。就稳定性而言,Nginx 比 lighthttpd 更胜一筹。

Nginx 支持热部署。它的启动特别容易, 并且几乎可以做到 7\*24 不间断运行，即使运行数个月也不需要重新启动。你还能够在不间断服务的情况下，对软件版本进行进行升级。

Nginx 采用 master-slave 模型,能够充分利用 SMP 的优势，且能够减少工作进程在磁盘 I/O 的阻塞延���。当采用 select()/poll() 调用时，还可以限制每个进程的连接数。

Nginx 代码质量非常高，代码很规范，手法成熟，模块扩展也很容易。特别值得一提的是强大的 Upstream 与 Filter 链。Upstream 为诸如 reverse proxy,与其他服务器通信模块的编写奠定了很好的基础。而 Filter 链最酷的部分就是各个 filter 不必等待前一个 filter 执行完毕。它可以把前一个 filter 的输出做为当前 filter 的输入，���有点像 Unix 的管线。这意味着，一个模块可以开始压缩从后端服务器发送过来的请求，且可以在模块接收完后端服务器的整个请求之前把压缩流转向客户端。

Nginx 采用了一些 os 提供的最新特性如对 sendfile (Linux2.2+)，accept-filter (FreeBSD4.1+)，TCP\_DEFER\_ACCEPT (Linux 2.4+)的支持，从而大大提高了性能。

当然，Nginx 还很年轻，多多少少存在一些问题，比如：Nginx 是俄罗斯人创建，虽然前几年文档比较少，但是目前文档方面比较全面，英文资料居多，中文的资料也比较多，而且有专门的书籍和资料可供查找。

Nginx 的作者和社区都在不断的努力完善，我们有理由相信 Nginx 将继续以高速的增长率来分享轻量级 HTTP 服务器市场，会有一个更美好的未来。


[nginx]: /pro/os/crawler/ZQBM-YIUY-FNUM.jpg
 *  **原文作者：** 程序猿阿锐
 *  **原文链接：** https://www.toutiao.com/item/6516296926294442509/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。