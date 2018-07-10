---
title: WebSocket系列之并发测试工具篇
date: 2017-10-12 22:05:27
categories: "开发"
tags:
	- 技术

---

![WebSocket系列之并发测试工具篇][WebSocket]

老皇叔致力于传播各种“姿势”

前言：WebSocket技术应用特别广泛，并发测试必不可少；前日偶然在网上看到在线测试和其作者提供的并发测试工具，使用后觉得其并发测试工具只支持IP+端口方式，有点不够用，故而提供URI的方式供并发测试使用。

![WebSocket系列之并发测试工具篇][WebSocket 1]

在线测试和并发压力测试工具

今日联系其作者，已告知当前在线版本中并发压力测试工具仅仅支持ip+端口，故而提供Java版实现，当前实现使用第三方Java-WebSocket-1.3.4为基础。使用方式如下：

# WebSocket并发测试工具使用说明 #

1、第一步：双击run.bat文件

![WebSocket系列之并发测试工具篇][WebSocket 2]

打开bat文件

2、正常打开前提是有java **jdk****环境**，打开界面如下：

![WebSocket系列之并发测试工具篇][WebSocket 3]

初始打开界面

3、输入ws 对应地址 和 并发测试数量，输出结果如下：击此处输入图片描述

![WebSocket系列之并发测试工具篇][WebSocket 4]

点我索要资源

4、查看ws服务端接收结果如下：

![WebSocket系列之并发测试工具篇][WebSocket 5]

结果界面

# 核心源码及相关类 #

 *  第三方jar包：Java-WebSocket-1.3.4，是基于Java语言的客户端实现(Java-WebSocket)
 *  EWebSocketClient类，是对WebSocketClient类的二次封装和扩展，简化对WebSocket的使用操作。

> package com.hoo.framework.ws;
> 
> import java.net.URI;
> 
> import java.util.ArrayList;
> 
> import java.util.Collections;
> 
> import java.util.Iterator;
> 
> import java.util.List;
> 
> import org.java\_websocket.client.WebSocketClient;
> 
> import org.java\_websocket.drafts.Draft\_17;
> 
> import org.java\_websocket.handshake.ServerHandshake;
> 
> /\*\*
> 
> \* WebSocketClient 扩展封装实现类\[暂支持WS\]
> 
> \* @author Hank
> 
> \*/
> 
> @SuppressWarnings("deprecation")
> 
> public class EWebSocketClient \{
> 
> private boolean socketOpen = false;
> 
> private List<String> messages = Collections.synchronizedList(new ArrayList<String>());
> 
> private SocketClientListener socketClientListener;
> 
> private WebSocketClient client;
> 
> public EWebSocketClient(URI uri)\{
> 
> client = new WebSocketClient(uri, new Draft\_17()) \{
> 
> @Override public void onClose(int arg0, String arg1, boolean arg2) \{
> 
> socketOpen = false;
> 
> if(null != socketClientListener)\{ socketClientListener.onClose(); \}
> 
> \}
> 
> @Override public void onError(Exception e) \{
> 
> socketOpen = false;
> 
> if(null != socketClientListener)\{ socketClientListener.onError(e); \}
> 
> \}
> 
> @Override public void onMessage(String message) \{
> 
> if(null != socketClientListener)\{ socketClientListener.onMessage(message); \}
> 
> \}
> 
> @Override public void onOpen(ServerHandshake arg0) \{
> 
> socketOpen = true;
> 
> sendMessages();
> 
> \}
> 
> \};
> 
> \}
> 
> public void connect()\{
> 
> client.connect();
> 
> \}
> 
> public void send(String message)\{
> 
> messages.add(message);
> 
> if(socketOpen)\{ sendMessages(); \}
> 
> \}
> 
> private void sendMessages()\{
> 
> if(client.isOpen())\{
> 
> synchronized (messages) \{
> 
> Iterator<String> msg = messages.iterator();
> 
> while (msg.hasNext())\{
> 
> client.send(msg.next());
> 
> \}
> 
> messages.clear();
> 
> \}
> 
> \}
> 
> \}
> 
> /\*\*
> 
> \* 是否已开启
> 
> \* @return
> 
> \*/
> 
> public boolean isOpen()\{
> 
> return socketOpen && client.isOpen();
> 
> \}
> 
> /\*\*
> 
> \* 关闭连接
> 
> \*/
> 
> public void close()\{
> 
> if(socketOpen)\{ if(!client.isClosed() && !client.isClosing())\{ client.close(); \} \}
> 
> \}
> 
> /\*\*
> 
> \* 需要在connect前调用
> 
> \* @param socketClientListener
> 
> \*/
> 
> public void setSocketClientListener(SocketClientListener socketClientListener) \{
> 
> this.socketClientListener = socketClientListener;
> 
> \}
> 
> public static abstract class SocketClientListener\{
> 
> public void onMessage(String message)\{\}
> 
> public void onClose()\{\}
> 
> public void onError(Exception e)\{\}
> 
> \}
> 
> \}

 *  WebSocketClientPool连接池类，目前比较简单，后续会更正优化。

![WebSocket系列之并发测试工具篇][WebSocket 6]

Pool上半部分截图

![WebSocket系列之并发测试工具篇][WebSocket 7]

Pool下半部分截图

 *  ConcurrentTesting类，用于控制并发请求的入口方法类。

结尾：

1.  索要所有资源请留言。
2.  感谢关注，不定期发布个人技术文章。
    


[WebSocket]: /pro/os/crawler/QUYF-QMBQ-N63M.jpg
[WebSocket 1]: /pro/os/crawler/EB6Z-6VZE-EMNY.jpg
[WebSocket 2]: /pro/os/crawler/EVZF-REUI-Q7JA.jpg
[WebSocket 3]: /pro/os/crawler/MVIU-2EM6-REQA.jpg
[WebSocket 4]: /pro/os/crawler/N7B6-FJJN-YRRF.jpg
[WebSocket 5]: /pro/os/crawler/ZBJ7-32MZ-QJ2E.jpg
[WebSocket 6]: /pro/os/crawler/QNUR-7NBV-E6NN.jpg
[WebSocket 7]: /pro/os/crawler/3QEZ-3UBE-3EE2.jpg
