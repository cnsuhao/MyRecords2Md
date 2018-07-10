---
title: 微信——高效解决token及授权用户openid的持久化处理办法
date: 2017-10-07 16:58:59
categories: "开发"
tags:
	- NoSQL
	- 移动互联网
	- 编程语言
	- 微信
	- Redis

---

摘要

关于微信开发的话题，例子确实已经有不少，但大部分都是人云亦云，很多小细节或者需要注意的地方却大多没有讲清楚，这令很多刚开始开发的人感觉大很迷茫。而我今天要说的话题，主要着眼于两个方面。

一：如何存储获取用户信息及调用第三方接口所需要的token.

二 : 第三方页面授权,如何减少从微信服务器获取用户openid的次数以及减少获取用户信息的次数，加速第三方页面的加载速速。

(注：演示所使用的是java语言，其他语言可与此类似)

--------------------

下面我将开始讲述第一个问题。

如何存储获取用户信息及调用第三方接口所需要的token？

从微信的官方文档上，我们知道，获取token的次数为1天2000次，每两小时token失效一次，对于那种一天没有几个人访问的微信公众号而言，他们可能只是简单的每次调用高级接口都会去获取一遍token.众所周知，这种做法极其的耗费时间。从网上其他网友给出的存储方案大概有如下几种：

1.  数据库：通过微信接口获取到 Token 之后，将 Token存储到数据库，每次需要时从数据库取出。采用定时任务的方法每隔一个固定的时间去获取一次token.
2.  NoSQl：这里以 Redis 为例子。通过微信接口获取到 Token 之后，存入 Redis，可以通过设置redis的过期时间，每次需要token时从redis中取出来，若没有，则证明Token 已过期可重新获取（当然也可采用上面的定时任务的方式定期获取）。
3.  文件存储：这个比较适合单一公众号的情况。通过微信接口获取到 Token 之后，存入文件，采用定时任务的方法每隔一个固定的时间去获取一次token.

（固定的时间：一般设为1小时为宜，如1小时后因网络原因，请求获取token失败，则原有的token还可以在使用1小时，这种方式将错误降低了一半）

大致有这三种方案。当然对于那些将token存储在session或者cookie里面的，这里我只能呵呵一笑了。

基于以上三种方式，我个人比较推崇的一个顺序是NoSQl>数据库>文件存储。

下面我分别来说说他们的优缺点：

采用数据库和文件存储，对于单节点并且用户请求数不是很多的web项目而言，是可以的正常运行的，但是对于分布式多节点的项目，采用这两种方式是行不通的。而我今天要说的第三种通过Redis方法存储，一方面它可以提升获取token的速度，另一方面当分布式项目的多个节点要公用同一个token的时候，我们可以方便的取到。

第三方页面授权,如何减少从微信服务器获取用户openid的次数以及减少获取用户信息的次数，加速第三方页面的加载速速？

从诸多的博客中，我们了解到，第三方页面授权获取用户信息，我们要调用两次微信接口。

 *  第一次：构造应用授权的url,通过返回的code，换取用户的openid.
 *  第二次：通过用户的openid与token获取用户信息。

对于页面比较多的应用，每个页面请求时都需要调用两个方法，于用户而言这是极其耗费时间的。

然而诸多的博客只是告诉我们改如何处理用户信息，诸如将用户信息先拉取下来存储到自己的数据库，然后每次需要时从自己的数据库中通过openid来获取。殊不知这种博客只说了一点，他们没有考虑到的问题是：

1.  用户若修改了自己的信息，该如何同步到自己的表里面.
2.  用户的信息是获取到了，但是每次用户访问网页，标识用户身份的openid依旧每次都要去调用接口获取。（获取用户信息的次数微信API规定为500000次）

那么接下来我要说的这个就是如何解决上面两个问题，处理过程大致如下:

a.添加拦截器，拦截需要授权页面的controller

拦截器:

> package com.fdc.home.dec.wx.filter;
> 
> import com.alibaba.dubbo.common.utils.StringUtils;
> 
> import com.fdc.platform.common.yfutil.PropertyReader;
> 
> import org.springframework.web.method.HandlerMethod;
> 
> import org.springframework.web.servlet.ModelAndView;
> 
> import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;
> 
> import javax.servlet.http.Cookie;
> 
> import javax.servlet.http.HttpServletRequest;
> 
> import javax.servlet.http.HttpServletResponse;
> 
> import java.util.Arrays;
> 
> /\*\*
> 
> \* Created by pl on 2017/1/13.
> 
> \*/
> 
> public class OAuth2Interceptor extends HandlerInterceptorAdapter \{
> 
> public static String indexUrl = PropertyReader.getValue("indexUrl");//从配置文件中读取域名
> 
> // private static String\[\] arrQueController = \{"newquestion", "mycenter","testCookie"\};
> 
> public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
> 
> throws Exception \{
> 
> Cookie\[\] cookies = request.getCookies();
> 
> String openId=null;
> 
> //判断cookie中是否存在openid 若存在则直接跳过，不存在则获取一次
> 
> if(cookies!=null)\{
> 
> for(Cookie cookie : cookies)\{
> 
> if(cookie.getName().equals("openId"))\{
> 
> openId = cookie.getValue();
> 
> \}
> 
> \}
> 
> \}
> 
> if (StringUtils.isEmpty(openId)) \{
> 
> if (handler instanceof HandlerMethod) \{
> 
> HandlerMethod handlerMethod = (HandlerMethod) handler;
> 
> String methodName = handlerMethod.getMethod().getName();
> 
> String uri = request.getRequestURI();
> 
> // if (checkList(arrQueController, methodName)) \{
> 
> // System.out.println("执行了");
> 
> response.sendRedirect(indexUrl + "/oauth2Api?resultUrl=" + indexUrl + uri);
> 
> return false;
> 
> // \}
> 
> \}
> 
> return true;
> 
> \} else \{
> 
> return true;
> 
> \}
> 
> \}
> 
> public void postHandle(
> 
> HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)
> 
> throws Exception \{
> 
> \}
> 
> public void afterCompletion(
> 
> HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
> 
> throws Exception \{
> 
> \}
> 
> public boolean checkList(String\[\] arr, String targetValue) \{
> 
> return Arrays.asList(arr).contains(targetValue);
> 
> \}
> 
> \}

spring-servlet.xml (拦截两个指定的controller：testCookie，testCookie1)  


> <mvc:interceptors>
> 
> <mvc:interceptor>
> 
> <mvc:mapping path="/testCookie"/>
> 
> <mvc:mapping path="/testCookie1"/>
> 
> <bean class="com.fdc.home.dec.wx.filter.OAuth2Interceptor"/>
> 
> </mvc:interceptor>
> 
> </mvc:interceptors>

b.用户首次访问，调用微信接口获取openid和用户信息。将openid写入服务器端的cookie,将用户的信息写入redis缓存中，以openid作为redis的key。

> package com.fdc.home.dec.wx.controller;
> 
> import com.fdc.home.dec.service.inter.service.DecWxService;
> 
> import com.fdc.home.dec.wx.service.CheckUserInfo;
> 
> import com.fdc.home.dec.wx.utils.JSONHelper;
> 
> import com.fdc.home.dec.wx.utils.WxUtils;
> 
> import com.fdc.home.dec.wx.vo.token.WeixinOauth2Token;
> 
> import com.fdc.home.dec.wx.vo.user.WeixinUserInfo;
> 
> import com.fdc.platform.common.yfutil.PropertyReader;
> 
> import org.springframework.beans.factory.annotation.Autowired;
> 
> import org.springframework.stereotype.Controller;
> 
> import org.springframework.web.bind.annotation.RequestMapping;
> 
> import org.springframework.web.bind.annotation.RequestParam;
> 
> import javax.servlet.http.Cookie;
> 
> import javax.servlet.http.HttpServletRequest;
> 
> import javax.servlet.http.HttpServletResponse;
> 
> import javax.servlet.http.HttpSession;
> 
> import java.io.IOException;
> 
> /\*\*
> 
> \* Created by pl on 2017/1/13.
> 
> \*/
> 
> @Controller
> 
> public class OAuth2Controller \{
> 
> @Autowired
> 
> private DecWxService decWxService;
> 
> //判断用户是否登录的公用方法
> 
> CheckUserInfo checkUserInfo = new CheckUserInfo();
> 
> //从配置文件获取appid
> 
> public static String appid = PropertyReader.getValue("appid");
> 
> //从配置文件获取appsecret
> 
> public static String appsecret = PropertyReader.getValue("appsecret");
> 
> //从配置文件获取主域名
> 
> public static String indexUrl = PropertyReader.getValue("indexUrl");
> 
> public static String wtoken = PropertyReader.getValue("wholetoken");
> 
> /\*\*
> 
> \* 组装授权url
> 
> \* @param request
> 
> \* @param resultUrl
> 
> \* @return
> 
> \*/
> 
> @RequestMapping(value ="/oauth2Api")
> 
> public String oauth2API(HttpServletRequest request, @RequestParam String resultUrl) \{
> 
> String redirectUrl = "";
> 
> if (resultUrl != null) \{
> 
> String backUrl =indexUrl+"/oauth2MeUrl?oauth2url="+resultUrl;
> 
> //组装授权url
> 
> redirectUrl = WxUtils.oAuth2Url(appid, backUrl);
> 
> \}
> 
> return "redirect:" + redirectUrl;
> 
> \}
> 
> /\*\*
> 
> \* 获取用户信息
> 
> \* @param request
> 
> \* @param response
> 
> \* @param code
> 
> \* @param oauth2url
> 
> \* @return
> 
> \* @throws IOException
> 
> \*/
> 
> @RequestMapping(value = "/oauth2MeUrl")
> 
> public String oauth2MeUrl(HttpServletRequest request,HttpServletResponse response, @RequestParam String code, @RequestParam String oauth2url) throws IOException \{
> 
> request.setCharacterEncoding("utf-8");
> 
> response.setCharacterEncoding("utf-8");
> 
> HttpSession session = request.getSession();
> 
> session.setAttribute("code",code);
> 
> // 用户同意授权
> 
> if (!"authdeny".equals(code)) \{
> 
> // 获取网页授权access\_token
> 
> WeixinOauth2Token weixinOauth2Token = WxUtils.getOauth2AccessToken(appid, appsecret, code);
> 
> // 网页授权接口访问凭证
> 
> String accessToken = weixinOauth2Token.getAccessToken();
> 
> // 用户标识
> 
> String openId = weixinOauth2Token.getOpenId();
> 
> String wholetoken = decWxService.getToken(wtoken);
> 
> //获取微信用户openid存储在cookie中的信息
> 
> Cookie userCookie=new Cookie("openId",openId);
> 
> userCookie.setMaxAge(-1);
> 
> userCookie.setPath("/");
> 
> response.addCookie(userCookie);
> 
> WeixinUserInfo weixinUserInfo = WxUtils.getWeixinUserInfo(wholetoken, openId);
> 
> //将用户信息写入redis
> 
> decWxService.setToken(openId, JSONHelper.beanToJson(weixinUserInfo));
> 
> \}else \{
> 
> return "redirect:"+indexUrl+"/error404";
> 
> \}
> 
> return "redirect:" + oauth2url;
> 
> \}
> 
> \}

以上两步基本可以解决用户授权的问题。基于此需要说明的是:

(注：WxUtils中封装各种请求微信服务器的接口，具体可自行百度)

以上两步基本可以解决用户授权的问题。基于此需要说明的是:

 *  开发中要设置cookie过期时间，设置为负数，表明当用户关闭浏览器的时候自动清空cookie,但在实际的测试中，微信浏览器并不会立刻清理cookie，你可以自行清理cookie.每次用户访问时直接从cookie中获取openid，若没有，才会调用微信接口获取。在获取openid的同时，更新redis缓存中的用户信息，这样达到及时同步用户信息的效果，也减少了对微信服务器的访问。
 *  有部分网页在博客中提到微信浏览器没有cookie和session,基于这类问题，还请自己动手验证吗，微信浏览器是有cookie和session的（请区分服务端session和客户端session）,这里之所以没有将openid存储在session中，其主要是考虑到分布式多点项目中session比较难以处理。
 *  比较重要的一点是，当用户更换微信登录时，cookie会自动清除，登录成功后，会重新获取新登录的用户的openid。

--------------------

# 如果你还需要了解更多技术文章信息，请继续关注博客园白衣秀才的博客。 #

  
  

 *  **原文作者：** 猿之家
 *  **原文链接：** https://www.toutiao.com/item/6474086476077335053/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。