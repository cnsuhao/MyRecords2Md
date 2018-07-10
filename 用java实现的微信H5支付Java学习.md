---
title: 用java实现的微信H5支付
date: 2017-03-30 18:58:48
categories: "开发"
tags:
	- Java
	- 移动互联网
	- 编程语言
	- 微信
	- HTML

---

前面做了app微信支付的回调处理，现在需要做微信公众号的支付，花了一天多时间，终于折腾出来了！鉴于坑爹的微信官方没有提供Java版的demo,所以全靠自己按照同样坑爹的文档敲敲敲，所以记录下来，以供自己及后来人参考，不足之处，还请指正。

首先，我们贴出调用支付接口的H5页面,当然，在这个页面之前，还需要做很多其他的操作，我们一步一步的来。

坑爹的官方文档给了两个不同的支付接口，在微信公众平台开发中文档的“微信JS-SDK说明文档”中，给出的支付方式是下面被屏蔽的那一部分，而在商户平台的“H5调起支付API”中，又给了一份不同的接口，即下面未屏蔽正常使用的接口。关于坑爹的微信提供了两个不同的支付接口，网上搜索结果也是众说纷纷，所以，只有自己试了。当然，为了简单，我直接试了下面这一种，然后奇迹般的成功了。

如果你想学习和了解更多关于java的知识点，可以来这个群，首先是一二六，中间是五三四，最后是五一九，里面有大量的学习资料可以下载。

<%@ page language="java" contentType="text/html; charset=UTF-8"

pageEncoding="UTF-8"%>

<!DOCTYPE html>

<html>

<head>

<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<title>微信网页支付</title>

<!-- -->

<!-- -->

<script type="text/javascript">

/\* wx.config(\{

debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。

appId: appid, // 必填，公众号的唯一标识

timestamp: timestamp, // 必填，生成签名的时间戳

nonceStr: nonceStr, // 必填，生成签名的随机串

signature: '',// 必填，签名，见附录1

jsApiList: \[chooseWXPay\] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2

\}); \*/

// config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作

//所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行

//wx.ready(function()\{

//参数是后台传过来的，签名加密，随机数，时间戳等全部后台处理好

var appId="$\{appId\}";

var timeStamp="$\{timeStamp\}";

var nonceStr="$\{nonceStr\}";

var prepay\_id="$\{prepay\_id\}";//之前参数名叫package,对应api接口，因为package是关键字，被坑了一次

var sign="$\{paySign\}";

//支付接口

function onBridgeReady()\{

WeixinJSBridge.invoke(

'getBrandWCPayRequest', \{

"appId" : appId, //公众号名称，由商户传入

"timeStamp" : timeStamp, //时间戳，自1970年以来的秒数 (java需要处理成10位才行，又一坑)

"nonceStr" : nonceStr, //随机串

"package" : prepay\_id, //拼装好的预支付标示

"signType" : "MD5",//微信签名方式

"paySign" : sign //微信签名

\},

function(res)\{

//使用以下方式判断前端返回,微信团队郑重提示：res.err\_msg将在用户支付成功后返回 ok，但并不保证它绝对可靠。

if(res.err\_msg == "get\_brand\_wcpay\_request:ok" ) \{

alert("支付成功");

\}else\{

alert("支付失败");

\}

\}

);

\}

if (typeof(WeixinJSBridge) == "undefined")\{

if( document.addEventListener )\{

document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);

\}elseif (document.attachEvent)\{

document.attachEvent('WeixinJSBridgeReady', onBridgeReady);

document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);

\}

\}else\{

onBridgeReady();

\}

//\});

//究竟哪个是支付接口

/\* wx.chooseWXPay(\{

timestamp: timestamp, // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符

nonceStr: nonceStr, // 支付签名随机串，不长于 32 位

package: 'prepay\_id', // 统一支付接口返回的prepay\_id参数值，提交格式如：prepay\_id=\*\*\*）

signType: 'MD5', // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'

paySign: sign, // 支付签名

success: function (res) \{

// 支付成功后的回调函数

//使用以下方式判断前端返回,微信团队郑重提示：res.err\_msg将在用户支付成功后返回 ok，但并不保证它绝对可靠。

if(res.err\_msg == "get\_brand\_wcpay\_request:ok" ) \{

\}

\}

\}); \*/

</script>

</head>

<body>

</body>

</html>

上面h5页面中，支付接口所需的参数都是由后台传过来的，除此之外，在进行上面一步之前，我们还需要获取一个预支付标识，下面贴上后台传参，及获取预支付标识和参数加密等方法（获取预支付标识之前需要网页授权获取用户openid，鉴于这个比较简单，所以不贴代码了）

首先是后台参数封装并对其签名（关键部分代码）：

if(payway.equals("1"))\{

System.out.println("----------支付宝支付-------------");

request.setAttribute("WIDout\_trade\_no", WIDout\_trade\_no);//订单号

request.setAttribute("WIDsubject", WIDsubject);//订单名称

request.setAttribute("WIDtotal\_fee", WIDtotal\_fee);//付款金额

request.setAttribute("WIDshow\_url", WIDshow\_url);//商品链接

request.setAttribute("WIDbody", "");//商品描述，可空

return "alipayapi";

\}elseif(payway.equals("2"))\{

System.out.println("----------微信支付-------------");

//1、通过网页授权接口，获取到的openid

String openid=request.getSession().getAttribute("openid")+"";

//处理价格单位为：分(请自行处理)

WIDtotal\_fee="1";

String preid=getPrepayid(WIDout\_trade\_no, WIDtotal\_fee, openid);//获取预支付标示

System.out.println("预支付标示：----------------"+preid);

//APPID

String appId=Common.appid;

request.setAttribute("appId", appId);

//时间戳

String timeStamp=(System.currentTimeMillis()/1000)+"";

request.setAttribute("timeStamp", timeStamp);

//随机字符串

String nonceStr=Common.randString(16).toString();

request.setAttribute("nonceStr", nonceStr);

//预支付标识

request.setAttribute("prepay\_id", "prepay\_id="+preid);

//加密方式

request.setAttribute("signType", "MD5");

//组装map用于生成sign

Map<String, String> map=new HashMap<String, String>();

map.put("appId", appId);

map.put("timeStamp", timeStamp);

map.put("nonceStr", nonceStr);

map.put("package", "prepay\_id="+preid);

map.put("signType", "MD5");

request.setAttribute("paySign", Common.sign(map, Common.MchSecret));//签名

return "weixinpay";

\}else \{

return "error";

\}

接下是获取预支付标识的方法getPrepayid:

/\*\*

\* 微信统一下单接口,获取预支付标示prepay\_id

\* @param out\_trade\_no1 商户订单号

\* @param total\_fee1 订单金额(单位:分)

\* @param openid1 网页授权取到的openid

\* @return

\*/

@ResponseBody

public String getPrepayid(String out\_trade\_no1,String total\_fee1,String openid1)\{

String result = "";

String appid = Common.appid;

String mch\_id = Common.mch\_id;

String nonce\_str = Common.randString(16);//生成随机数，可直接用系统提供的方法

String body = "E光学-商品订单";

String out\_trade\_no = out\_trade\_no1;

String total\_fee = total\_fee1;

String spbill\_create\_ip = "xxx.xxx.38.47";//用户端ip,这里随意输入的

String notify\_url = "网页链接//支付回调地址

String trade\_type = "JSAPI";

String openid = openid1;

HashMap<String, String> map = new HashMap<String, String>();

map.put("appid", appid);

map.put("mch\_id", mch\_id);

map.put("attach", "支付测试");

map.put("device\_info", "WEB");

map.put("nonce\_str", nonce\_str);

map.put("body", body);

map.put("out\_trade\_no", out\_trade\_no);

map.put("total\_fee", total\_fee);

map.put("spbill\_create\_ip", spbill\_create\_ip);

map.put("trade\_type", trade\_type);

map.put("notify\_url", notify\_url);

map.put("openid", openid);

String sign = Common.sign(map, Common.MchSecret);//参数加密

System.out.println("sign秘钥:-----------"+sign);

map.put("sign", sign);

//组装xml(wx就这么变态，非得加点xml在里面)

String content=Common.MapToXml(map);

//System.out.println(content);

String PostResult=HttpClient.HttpsPost("网页链接);

JSONObject jsonObject=XmlUtil.XmlToJson(PostResult);//返回的的结果

if(jsonObject.getString("return\_code").equals("SUCCESS")&&jsonObject.getString("result\_code").equals("SUCCESS"))\{

result=jsonObject.get("prepay\_id")+"";//这就是预支付id

\}

return result;

\}

接下是签名的方法（MD5加密是调用微信一个jar里面的，你也可以自己写一个，网上很多参考）：

Map转XML的方法：

以上就是java实现微信H5支付的主要代码了，大部分都有注释，也没有什么好解释的了。当然，仅供参考，仅供参考，仅供参考！！！
 *  **原文作者：** Java学习
 *  **原文链接：** https://www.toutiao.com/item/6403244456551645698/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。