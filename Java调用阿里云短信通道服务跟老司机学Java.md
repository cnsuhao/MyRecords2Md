---
title: Java调用阿里云短信通道服务
date: 2018-01-29 10:52:19
categories: "开发"
tags:
	- Java
	- jQuery
	- 编程语言
	- JSON
	- 澳洲航空

---

这里我们使用SpringBoot 来调用阿里通信的服务。

阿里通信，双11.收到短信,日发送达6亿条。保障力度非常高。

使用的步骤：

**第一步：需要开通账户**

![Java调用阿里云短信通道服务][Java]

**第二步：阅读接口文档**

![Java调用阿里云短信通道服务][Java 1]

**秘钥管理**

![Java调用阿里云短信通道服务][Java 2]

**短信签名**

![Java调用阿里云短信通道服务][Java 3]

**短信模板**

![Java调用阿里云短信通道服务][Java 4]

**SDK**

这个由阿里云提供。

![Java调用阿里云短信通道服务][Java 5]

编译与打包。

打包到本地仓库，或者公司局域网内的私服地址。

Maven打包

**第三步：创建SpringBoot工程，导入依赖**

![Java调用阿里云短信通道服务][Java 6]

![Java调用阿里云短信通道服务][Java 7]

**第四步：准备页面**

关注内容：

``````````
<script src="mtlogin/jquery-2.1.0.js"></script><script type="text/javascript"> function getNum(){ $.ajax({ type: "POST", url: "ajaxNum", data: "phoneNum="+document.getElementById("login-mobile").value, success: function(msg){ document.getElementById("login-verify-code").value=msg; } }); }</script>
``````````

**第五步：调用阿里通信接口**

核心代码：

**package** com.qf.action;

**import** com.aliyuncs.DefaultAcsClient;

**import** com.aliyuncs.IAcsClient;

**import** com.aliyuncs.dysmsapi.model.v20170525.SendSmsRequest;

**import** com.aliyuncs.dysmsapi.model.v20170525.SendSmsResponse;

**import** com.aliyuncs.exceptions.ClientException;

**import** com.aliyuncs.http.MethodType;

**import** com.aliyuncs.profile.DefaultProfile;

**import** com.aliyuncs.profile.IClientProfile;

**import** com.qf.utils.AliAccessKey;

**import** com.qf.utils.RandomStringTLUtils;

**import** org.springframework.stereotype.Controller;

**import** org.springframework.web.bind.annotation.RequestMapping;

**import** org.springframework.web.bind.annotation.ResponseBody;

/\*\*

\* Thanks for Everything.

\*/

@Controller

**public class** SmsAction \{

//显示页面

@RequestMapping(**"/mt"**)

**private** String ui()\{

**return "mtlogin"**;//返回页面

\}

@RequestMapping(**"/ajaxNum"**)

@ResponseBody

**public** String sendMsg(String phoneNum) **throws** ClientException \{//拿到手机号

//调用阿里通信接口

//设置超时时间-可自行调整

System.setProperty(**"sun.net.client.defaultConnectTimeout"**, **"10000"**);

System.setProperty(**"sun.net.client.defaultReadTimeout"**, **"10000"**);

//初始化ascClient需要的几个参数

**final** String product = **"Dysmsapi"**;//短信API产品名称（短信产品名固定，无需修改）

**final** String domain = **"dysmsapi.aliyuncs.com"**;//短信API产品域名（接口地址固定，无需修改）

//替换成你的AK

**final** String accessKeyId = AliAccessKey.accessKeyId;//你的accessKeyId,参考本文档步骤2

**final** String accessKeySecret = AliAccessKey.accessKeySecret;//你的accessKeySecret，参考本文档步骤2

//初始化ascClient,暂时不支持多region（请勿修改）

IClientProfile profile = DefaultProfile.getProfile(**"cn-hangzhou"**, accessKeyId,

accessKeySecret);

DefaultProfile.addEndpoint(**"cn-hangzhou"**, **"cn-hangzhou"**, product, domain);

IAcsClient acsClient = **new** DefaultAcsClient(profile);

//组装请求对象

SendSmsRequest request = **new** SendSmsRequest();

//使用post提交

request.setMethod(MethodType.**POST**);

//必填:待发送手机号。支持以逗号分隔的形式进行批量调用，批量上限为1000个手机号码,批量调用相对于单条调用及时性稍有延迟,验证码类型的短信推荐使用单条调用的方式

request.setPhoneNumbers(phoneNum);

//必填:短信签名-可在短信控制台中找到

request.setSignName(**"**短信签名名称**"**);

//必填:短信模板-可在短信控制台中找到

request.setTemplateCode(**"**短信模板**"**);

//可选:模板中的变量替换JSON串,如模板内容为"亲爱的$\{name\},您的验证码为$\{code\}"时,此处的值为

//友情提示:如果JSON中需要带换行符,请参照标准的JSON协议对换行符的要求,比如短信内容中包含 的情况在JSON中需要表示成\\r\\n,否则会导致JSON在服务端解析失败

//生成几位的验证码

String numeric = RandomStringTLUtils.randomNumeric(6);

request.setTemplateParam(**"\{"code":""**\+numeric+**""\}"**);

//可选-上行短信扩展码(扩展码字段控制在7位或以下，无特殊需求用户请忽略此字段)

//request.setSmsUpExtendCode("90997");

//可选:outId为提供给业务方扩展字段,最终在短信回执消息中将此值带回给调用者

request.setOutId(**"qf"**);

//请求失败这里会抛ClientException异常

SendSmsResponse sendSmsResponse = acsClient.getAcsResponse(request);

**if**(sendSmsResponse.getCode() != **null** && sendSmsResponse.getCode().equals(**"OK"**)) \{

//请求成功

//真实应用的时候验证码在服务端有记录

//客户端由客户来输入

//客户输入的验证码和服务端做匹配

**return** numeric;

\}

**return "error"**;

\}

\}

**第六步：测试**

![Java调用阿里云短信通道服务][Java 8]

注意：需要收费。


[Java]: /pro/os/crawler/UBBI-NVYM-ZFNM.jpg
[Java 1]: /pro/os/crawler/RRB3-UYQN-6BN3.jpg
[Java 2]: /pro/os/crawler/YUE6-FEQE-ERFA.jpg
[Java 3]: /pro/os/crawler/UABR-R2QI-UYIB.jpg
[Java 4]: /pro/os/crawler/QQQQ-RUAI-JQAR.jpg
[Java 5]: /pro/os/crawler/EQRR-YU6B-YVVA.jpg
[Java 6]: /pro/os/crawler/AMRU-ZUBQ-QYYN.jpg
[Java 7]: /pro/os/crawler/RJIA-2IEN-AJUE.jpg
[Java 8]: /pro/os/crawler/ZUYB-RNZB-FBQZ.jpg
 *  **原文作者：** 跟老司机学Java
 *  **原文链接：** https://www.toutiao.com/item/6516300067744252419/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。