---
title: Titan Framework 如何编写Restful API
date: 2018-06-04 10:14:17
categories: "开发"
tags:
	- 技术
	- JSON

---

![Titan Framework 如何编写Restful API][Titan Framework _Restful API]

**介绍**

Titan Framework 针对Restful 风格基于Spring 进行结构封装，将请求转化为对应的Command(CreateCommand、PutCommand、DeleteCommand、PatchCommand、GetCommand)，因此针对API的定义只需要面向接收到的请求进行对象的转换，并返回相应的Command 。

**API的编写**

controller定义,系统默认接收Json body ，基于JsonToEntity模式进行对象转换，调用方法如下：JsonParserFactory.getParser().getObject(body, TplAppEntity.**class**);

@RestController

@RequestMapping("/demoa")

**public** **class** ADemoController **extends** RestfulController \{

/\*\* 系统默认HTTP POST方法\*\*/

@Override

**protected** <T> Command<?> getCreateCommand(String service, T body) \{

String \_body = (String) body;

DemoAEntity entity = JsonParser.getObject(\_body, DemoAEntity.**class**);

**return** **new** Create<DemoAEntity>(entity);

\}

/\*\* 系统默认**HTTP** Delete方法\*\*/

@Override

**protected** Command<?> getDeleteCommand(String service, String id) \{

DemoAEntity entity = **new** DemoAEntity();

entity.setId(id);

**return** **new** Delete<DemoAEntity>(entity);

\}

/\*\* 系统默认**HTTP** Get方法\*\*/

@Override

**protected** Command<?> getGetCommand(String service, String arg1) \{

**return** **null**;

\}

/\*\* 系统默认**HTTP** Patch方法\*\*/

@Override

**protected** Command<?> getPatchCommand(String arg0, String arg1, String arg2) \{

**return** **null**;

\}

/\*\* 系统默认**HTTP** Put方法\*\*/

@Override

**protected** Command<?> getPutCommand(String arg0, String arg1, String arg2) \{

**return** **null**;

\}

/\*\* 系统默认query方法\*\*/

@Override

**protected** Command<?> getQueryCommand(String arg0, Object arg1) \{

**return** **null**;

\}

\}

**Command Handler定义**

Command Handler是用来响应API (Controller的部分的请求），基于类上的注解在启动时自动完成handler与Command的注册过程。

@CmdHandler

**public** **class** DemoACreateHandler **implements** CommandHandler<Create<DemoAEntity>> \{

@Override

**public** Result handle(Create<DemoAEntity> cmd) \{

DemoAEntity entity = cmd.getEntity();

System.**out**.println(entity.getName() + "," + entity.getText());

Result result = Publish.Send(Ways.Remote(Paths.**DemoB**.service.getServerName(),

**new** DemoACreateEvent())).Retry();

**return** result;

\}

\}

**Titan Framework的 Command模型**

Titan Framework通过Command将Controller与对应的CommandHandler进行关联。在上面的例子中，我们可以看到POST请求将会被getCreateCommand方法捕获，捕获后通过创建一个Create类型的Command将具体的请求内容提交到一个CommandHandler。

具体的绑定逻辑关键点在于Command类型以及Command中绑定的Entity类型。在上面的例子中，getCreateCommand提交了一个Create<DemoAEntity>的command。对应的CommandHandler（注意：必须通过@CmdHandler注释进行标注，否则Titan Framework无法有效识别）实现了一个CommandHandler<Create<DemoAEntity>>的接口。在这个情况下，Titan Framework会在底层通过Create<DemoAEntity>这个Command类型完成Controller层与CommandHandler层的匹配。

作为Titan Framework的高阶用法，我们在getCreateCommand中可以通过serviceName（getCreateCommand有两个参数，分别是serviceName与T body），分离出不同的分支，向多个不同的CommandHandler发送不同的Command。这一点，大家可以在实际使用中进一步探索。


[Titan Framework _Restful API]: /pro/os/crawler/NBZQ-REUV-IB7R.jpg
 *  **原文作者：** Titan框架
 *  **原文链接：** https://www.toutiao.com/item/6563045984450904584/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。