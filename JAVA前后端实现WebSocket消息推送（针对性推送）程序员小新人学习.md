---
title: JAVA前后端实现WebSocket消息推送（针对性推送）
date: 2018-06-24 11:47:11
categories: "开发"
tags:
	- Java
	- jQuery
	- 编程语言
	- JSON
	- HTML

---

1、需要添加依赖包，在pom.xml文件中添加

**\[java\]** view plain copy

1.  
2.  <dependency>
3.  <groupId>javax</groupId>
4.  <artifactId>javaee-api</artifactId>
5.  <span style="white-space:pre;"> </span><version>7.0</version>
6.  <span style="white-space:pre;"> </span><scope>provided</scope>
7.  </dependency>

2、客户端代码在这里我为了做成httpsession登录后是同一个，所以我做成两个页面，一个登录跳转页面，一个用于链接WebSocket接收消息

a.登录页面

**\[html\]** view plain copy

1.  <!DOCTYPE html**>**
2.  **<html>**
3.  
4.  **<head>**
5.  **<meta** charset="UTF-8"**>**
6.  **<title>**WebSocket**</title>**
7.  **<script** src="js/jquery-1.8.3.min.js"**></script>**
8.  **<script** type="text/javascript"**>**
9.  function dl() \{
10. $.ajax(\{
11. xhrFields: \{
12. withCredentials: true
13. \},
14. type:"get",
15. url:"http://localhost:8080/cloudmgr/api/login?user=ppp",
16. \});
17. \}
18. **</script>**
19. **</head>**
20. 
21. **<body>**
22. **<input** type="button" value="登录" onclick="dl()" **/>**
23. **<a** href="login.html"**>**tiaozhuan**</a>**
24. **</body>**
25. 
26. **</html>**

b.接收消息推送页面

**\[html\]** view plain copy

1.  <!DOCTYPE html**>**
2.  **<html>**
3.  
4.  **<head>**
5.  **<meta** charset="UTF-8"**>**
6.  **<title>**WebSocket**</title>**
7.  **<script** src="js/jquery-1.8.3.min.js"**></script>**
8.  **<script** type="text/javascript"**>**
9.  var ws = null;
10. //判断当前浏览器是否支持WebSocket
11. if('WebSocket' in window) \{
12. ws = new WebSocket("ws://localhost:8080/cloudmgr/chat");
13. \} else \{
14. alert('当前浏览器 Not support websocket')
15. \}
16. /\*
17. \*监听三种状态的变化js会回调
18. \*/
19. ws.onopen = function(message) \{
20. // 连接回调
21. \};
22. ws.onclose = function(message) \{
23. // 断开连接回调
24. \};
25. ws.onmessage = function(message) \{
26. // 消息监听
27. showMessage(message.data);
28. \};
29. //监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
30. window.onbeforeunload = function() \{
31. ws.close();
32. \};
33. //关闭连接
34. function closeWebSocket() \{
35. ws.close();
36. \}
37. //发送消息
38. function send() \{
39. 
40. var input = document.getElementById("msg");
41. var text = input.value;
42. 
43. // 消息体JSON 对象 对应JAVA 的 Msg对象
44. var data = \{
45. // 定点发送给其他用户的userId
46. toUid: "3d535429-5fcb-4490-bcf7-96fd84bb17b6",
47. data: text
48. \}
49. 
50. ws.send(JSON.stringify(data));
51. input.value = "";
52. \}
53. 
54. function showMessage(message) \{
55. /\*var text = document.createTextNode(JSON.parse(message).data);
56. var br = document.createElement("br")
57. var div = document.getElementById("showChatMessage");
58. div.appendChild(text);
59. div.appendChild(br);\*/
60. var text = document.createTextNode(message);
61. document.getElementById("showText").appendChild(text);
62. 
63. \}
64. **</script>**
65. **</head>**
66. 
67. **<body>**
68. **<div>**
69. **<style**style="width: 600px; height: 240px; overflow-y: auto; border: 1px solid \#333;" id="show"**>**
70. **<div** id="showChatMessage"**></div>**
71. **<div** id="showText"**/>**
72. **<input** type="text" size="80" id="msg" name="msg" placeholder="输入聊天内容" **/>**
73. **<input** type="button" value="发送" id="sendBn" name="sendBn" onclick="send()"**>**
74. **</div>**
75. **</body>**
76. 
77. **</html>**

3、关于后端代码这边事由4个文件

一个通用msg文件、一个用于获取当前会话的httpsession、一个用监听有没有httpsession（没有则创建）、一个用于WebSocket链接和发送消息

a.通用msg文件

**\[java\]** view plain copy

1.  **package** com.boli.srcoin.websocket;
2.  
3.  **import** java.util.Date;
4.  
5.  /\*\*
6.  \* @author : admin</br>
7.  \* @DESC : <p>WebSocket消息模型</p></br>
8.  \*/
9.  **public** **class** Msg \{
10. 
11. // 推送人ID
12. **private** String fromUid;
13. 
14. // 定点推送人ID
15. **private** String toUid;
16. 
17. // 定点推送单位ID
18. **private** String toOrgId;
19. 
20. // 消息体
21. **private** String data;
22. 
23. // 推送时间
24. **private** Date createDate = **new** Date();
25. 
26. // 消息状态
27. **private** Integer flag;
28. 
29. **public** Msg() \{
30. 
31. \}
32. 
33. **public** Msg(String fromUid, String toUid, String toOrgId, String data, Date createDate, Integer flag) \{
34. **this**.fromUid = fromUid;
35. **this**.toUid = toUid;
36. **this**.toOrgId = toOrgId;
37. **this**.data = data;
38. **this**.createDate = createDate;
39. **this**.flag = flag;
40. \}
41. 
42. **public** String getFromUid() \{
43. **return** fromUid;
44. \}
45. 
46. **public** **void** setFromUid(String fromUid) \{
47. **this**.fromUid = fromUid;
48. \}
49. 
50. **public** String getToUid() \{
51. **return** toUid;
52. \}
53. 
54. **public** **void** setToUid(String toUid) \{
55. **this**.toUid = toUid;
56. \}
57. 
58. **public** String getToOrgId() \{
59. **return** toOrgId;
60. \}
61. 
62. **public** **void** setToOrgId(String toOrgId) \{
63. **this**.toOrgId = toOrgId;
64. \}
65. 
66. **public** String getData() \{
67. **return** data;
68. \}
69. 
70. **public** **void** setData(String data) \{
71. **this**.data = data;
72. \}
73. 
74. **public** Date getCreateDate() \{
75. **return** createDate;
76. \}
77. 
78. **public** **void** setCreateDate(Date createDate) \{
79. **this**.createDate = createDate;
80. \}
81. 
82. **public** Integer getFlag() \{
83. **return** flag;
84. \}
85. 
86. **public** **void** setFlag(Integer flag) \{
87. **this**.flag = flag;
88. \}
89. 
90. @Override
91. **public** String toString() \{
92. **return** "Msg\{" +
93. "fromUid='" + fromUid + ''' +
94. ", toUid='" + toUid + ''' +
95. ", toOrgId='" + toOrgId + ''' +
96. ", data='" + data + ''' +
97. ", createDate=" + createDate +
98. ", flag=" + flag +
99. '\}';
100. \}
101. \}

b.用于在WebSocket或去httpsession

**\[java\]** view plain copy

1.  **package** com.boli.srcoin.websocket;
2.  
3.  **import** javax.servlet.http.HttpSession;
4.  **import** javax.websocket.HandshakeResponse;
5.  **import** javax.websocket.server.HandshakeRequest;
6.  **import** javax.websocket.server.ServerEndpointConfig;
7.  **import** javax.websocket.server.ServerEndpointConfig.Configurator;
8.  
9.  /\*\*
10. \* @author : admin</br>
11. \* @DESC : <p>讲http request的session 存入websocket的session内</p></br>
12. \*/
13. **public** **class** HttpSessionConfigurator **extends** Configurator \{
14. 
15. @Override
16. **public** **void** modifyHandshake(ServerEndpointConfig sec,
17. HandshakeRequest request, HandshakeResponse response) \{
18. 
19. // 获取当前Http连接的session
20. HttpSession httpSession = (HttpSession) request.getHttpSession();
21. // 将http session信息注入websocket session
22. sec.getUserProperties().put(HttpSession.**class**.getName(), httpSession);
23. \}
24. \}

c.用于监听有没有httpsession，没有则创建

**\[java\]** view plain copy

1.  **package** com.boli.srcoin.websocket;
2.  
3.  **import** javax.servlet.ServletRequestEvent;
4.  **import** javax.servlet.ServletRequestListener;
5.  **import** javax.servlet.annotation.WebListener;
6.  **import** javax.servlet.http.HttpServletRequest;
7.  
8.  @WebListener
9.  **public** **class** RequestListener **implements** ServletRequestListener \{
10. 
11. **public** **void** requestInitialized(ServletRequestEvent sre) \{
12. //将所有request请求都携带上httpSession
13. ((HttpServletRequest) sre.getServletRequest()).getSession();
14. 
15. \}
16. **public** RequestListener() \{
17. // TODO Auto-generated constructor stub
18. \}
19. 
20. **public** **void** requestDestroyed(ServletRequestEvent arg0) \{
21. // TODO Auto-generated method stub
22. \}
23. \}

d.接收WebSocket链接和发送消息**\[java\]** view plain copy

1.  **package** com.boli.srcoin.websocket;
2.  
3.  **import** com.alibaba.fastjson.JSON;
4.  **import** org.apache.commons.lang.StringUtils;
5.  **import** org.apache.log4j.Logger;
6.  
7.  **import** javax.servlet.http.HttpSession;
8.  **import** javax.websocket.\*;
9.  **import** javax.websocket.server.ServerEndpoint;
10. **import** java.io.IOException;
11. **import** java.util.concurrent.ConcurrentHashMap;
12. **import** java.util.concurrent.ConcurrentMap;
13. 
14. /\*\*
15. \* @author : admin</br>
16. \* @DESC : <p>注解\{@link ServerEndpoint\}声明websocket 服务端</p></br>
17. \*/
18. @ServerEndpoint(value = "/chat", configurator = HttpSessionConfigurator.**class**)
19. **public** **class** WSServer \{
20. 
21. **static** **private** Logger logger = Logger.getLogger(WSServer.**class**);
22. 
23. // 在线人数 线程安全
24. **private** **static** **int** onlineCount = 0;
25. 
26. // 连接集合 userId => server 键值对 线程安全
27. **static** **public** **final** ConcurrentMap<String, WSServer> map = **new** ConcurrentHashMap<>();
28. 
29. // 与某个客户端的连接会话，需要通过它来给客户端发送数据
30. **private** Session session;
31. 
32. // 当前会话的httpsession
33. **private** HttpSession httpSession;
34. 
35. 
36. /\*\*
37. \* @param session websocket连接sesson
38. \* @param config \{@link com.github.websocket.configuration.HttpSessionConfigurator\}
39. \* @DESC <p>注解\{@link OnOpen\} 声明客户端连接进入的方法</p>
40. \*/
41. @OnOpen
42. **public** **void** onOpen(Session session, EndpointConfig config) \{
43. 
44. // 得到httpSession
45. **this**.httpSession = (HttpSession) config.getUserProperties().get(HttpSession.**class**.getName());
46. 
47. // 获取session对象 SObject(这个就是java web登入后的保存的session对象，此处为用户信息，包含了userId)
48. String user = (String) **this**.httpSession.getAttribute("user");
49. 
50. **this**.session = session;
51. System.out.println(user+"-------"+**this**.session.getId());
52. 
53. //针对一个用户只能有一个链接
54. **if**(map.get(user)!=**null**)\{
55. // 移除连接
56. map.remove(user);
57. // 连接数-1
58. subOnlineCount();
59. \}
60. 
61. // 将连接session对象存入map
62. map.put(user, **this**);
63. 
64. // 连接数+1
65. addOnlineCount();
66. 
67. logger.info("有新的连接，当前连接数为：" + getOnlineCount());
68. \}
69. 
70. 
71. /\*\*
72. \* <p>\{@link OnClose\} 关闭连接</p>
73. \*/
74. @OnClose
75. **public** **void** onClose() \{
76. 
77. /\*\*
78. \* 获取当前连接信息 \{@code CommonConstant.USER\_LOGIN\_SESSION\} 为Http session 名
79. \*/
80. 
81. String user = (String) **this**.httpSession.getAttribute("user");
82. 
83. // 移除连接
84. map.remove(user);
85. 
86. // 连接数-1
87. subOnlineCount();
88. 
89. logger.info("有一连接断开，当前连接数为：" + getOnlineCount());
90. \}
91. 
92. /\*\*
93. \* <p>\{@link OnMessage\} 消息监听处理方法</p>
94. \*
95. \* @param message 消息对象\{@link com.github.websocket.msg.Msg\}的JSON对象
96. \* @throws IOException 异常
97. \*/
98. @OnMessage
99. **public** **void** onMessage(String message) **throws** IOException \{
100. 
101. // 将消息转Msg对象
102. Msg msg = JSON.parseObject(message, Msg.**class**);
103. 
104. //TODO 可以对msg做些处理...
105. 
106. // 根据Msg消息对象获取定点发送人的userId
107. WSServer \_client = map.get(msg.getToUid());
108. 
109. // 定点发送
110. **if** (StringUtils.isNotEmpty(msg.getToUid())) \{
111. **if** (**null** != \_client) \{
112. // 是否连接判断
113. **if** (\_client.session.isOpen())
114. // 消息发送
115. \_client.session.getBasicRemote().sendText(JSON.toJSONString(msg));
116. \}
117. \}
118. 
119. // 群发
120. **if** (StringUtils.isEmpty(msg.getToUid())) \{
121. // 群发已连接用户
122. **for** (WSServer client : map.values()) \{
123. client.session.getBasicRemote().sendText(JSON.toJSONString(msg));
124. \}
125. \}
126. 
127. \}
128. 
129. /\*\*
130. \* <p>\{@link OnError\} websocket系统异常处理</p>
131. \*
132. \* @param t 异常
133. \*/
134. @OnError
135. **public** **void** onError(Throwable t) \{
136. logger.error(t);
137. t.printStackTrace();
138. \}
139. 
140. /\*\*
141. \* <p>系统主动推送 这是个静态方法在web启动后可在程序的其他合适的地方和时间调用，这就实现了系统的主动推送</p>
142. \*
143. \* @param msg 消息对象\{@link com.github.websocket.msg.Msg\}的JSON对象
144. \*/
145. **static**
146. **public** **void** pushBySys(Msg msg) \{
147. 
148. //TODO 也可以实现定点推送
149. //msg传输的时候会带上需要发送消息给谁msg.getToUid()
150. //通过map去获取那个用户所在的session
151. WSServer ws=map.get(msg.getToUid());
152. **try** \{
153. **if**(ws!=**null**)\{
154. ws.session.getBasicRemote().sendText("123456");
155. \}
156. \} **catch** (IOException e1) \{
157. e1.printStackTrace();
158. \}
159. 
160. // 群发
161. /\*for (WSServer client : map.values()) \{
162. try \{
163. client.session.getBasicRemote().sendText(JSON.toJSONString(msg));
164. \} catch (IOException e) \{
165. e.printStackTrace();
166. \}
167. \}\*/
168. \}
169. 
170. // 获取连接数
171. **private** **static** **synchronized** **int** getOnlineCount() \{
172. **return** WSServer.onlineCount;
173. \}
174. 
175. // 增加连接数
176. **private** **static** **synchronized** **void** addOnlineCount() \{
177. WSServer.onlineCount++;
178. \}
179. 
180. // 减少连接数
181. **private** **static** **synchronized** **void** subOnlineCount() \{
182. WSServer.onlineCount--;
183. \}
184. 
185. \}

4、在后端的调用，也就是登录和调用发送消息

**\[java\]** view plain copy

1.  **package** com.boli.srcoin.member.service.impl;
2.  
3.  **import** java.util.HashMap;
4.  **import** java.util.Map;
5.  
6.  **import** org.springframework.stereotype.Service;
7.  **import** org.springframework.transaction.annotation.Transactional;
8.  
9.  **import** com.boli.framework.system.result.StandardResult;
10. **import** com.boli.framework.utils.WebUtil;
11. **import** com.boli.srcoin.member.form.MemberLoginForm;
12. **import** com.boli.srcoin.member.service.LoginMemberService;
13. **import** com.boli.srcoin.websocket.Msg;
14. **import** com.boli.srcoin.websocket.WSServer;
15. 
16. @Service
17. **public** **class** LoginMemberServiceImpl **implements** LoginMemberService\{
18. 
19. @Override
20. @Transactional(readOnly = **false**)
21. **public** StandardResult toLogin(MemberLoginForm loginForm) \{
22. WebUtil.getSession().setAttribute("user", loginForm.getUser());
23. Map<String,Object> map=**new** HashMap<>();
24. map.put("sessionId", WebUtil.getSession().getId());
25. map.put("user", loginForm.getUser());
26. System.out.println("调用登录方法："+WebUtil.getSession().getId()+loginForm.getUser());
27. **return** StandardResult.ok(map);
28. \}
29. 
30. @Override
31. @Transactional(readOnly = **false**)
32. **public** StandardResult tishi() \{
33. Msg msg=**new** Msg();
34. msg.setToUid("ppp");
35. WSServer.pushBySys(msg);
36. **return** StandardResult.ok();
37. \}
38. 
39. 
40. \}

5、调用结果如图

![JAVA前后端实现WebSocket消息推送（针对性推送）][JAVA_WebSocket]

![JAVA前后端实现WebSocket消息推送（针对性推送）][JAVA_WebSocket 1]

![JAVA前后端实现WebSocket消息推送（针对性推送）][JAVA_WebSocket 2]

附带源码链接：http://download.csdn.net/download/qq\_31151929/10207702


[JAVA_WebSocket]: /pro/os/crawler/VZIN-FANJ-MVFF.jpg
[JAVA_WebSocket 1]: /pro/os/crawler/FIZM-AQFN-J7JF.jpg
[JAVA_WebSocket 2]: /pro/os/crawler/ZRJR-VRVA-JEQF.jpg
 *  **原文作者：** 程序员小新人学习
 *  **原文链接：** https://www.toutiao.com/item/6570492644437262861/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。