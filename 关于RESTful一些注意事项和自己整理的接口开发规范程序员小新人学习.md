---
title: 关于RESTful一些注意事项和自己整理的接口开发规范
date: 2018-06-26 16:56:59
categories: "开发"
tags:
	- Java
	- XML
	- 编程语言
	- JSON
	- 动物

---

**最近在研究restful，公司开发要使用，所以自己就去网上找了好些资料，并整理了一套公司开发的接口规范。当然，我也只是刚刚入坑。还不是很全面。但是这就是一个过程。一点点，总会好起来的。以下是就是RESTful接口规范：**

**一、 URI**

**URI规范**

1.不用大写；

2.用中杠 **-** 不用下杠 **\_** ；

3.参数列表要encode；

4.URI中的名词表示资源集合，使用复数形式。

5.在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词（特殊情况可以使用动词），而且所用的名词往往与数据库的表格名对应。

**资源集合 vs单个资源**

URI表示资源的两种方式：资源集合、单个资源。

资源集合：

 **/zoos //所有动物园**

 **/zoos/1/animals //id为1的动物园中的所有动物**

单个资源：

 **/zoos/1//id为1的动物园**

 **/zoos/1;2;3//id为1，2，3的动物园**

**避免层级过深的URI**

在url中表达层级，用于 按实体关联关系进行对象导航 ，一般根据id导航。

过深的导航容易导致url膨胀，不易维护，如 GET /zoos/1/areas/3/animals/4 ，尽量使用查询参数代替路径中的实体导航，如 GET/animals?zoo=1&area=3 ；

**二、 版本**

**应该将API的版本号放入到URI中**

https://api.example.com/v1/zoos

**三、 Request**

**HTTP方法**

通过标准HTTP方法对资源CRUD：

GET：查询（从服务器取出资源一项或多项）

GET /zoos

GET /zoos/1

GET/zoos/1/employees

POST：创建单个新资源。 POST一般向“资源集合”型uri发起

POST/animals //新增动物

POST/zoos/1/employees //为id为1的动物园雇佣员工

PUT：更新单个资源（全量），客户端提供完整的更新后的资源。与之对应的是 PATCH，PATCH负责部分更新，客户端提供要更新的那些字段。 PUT/PATCH一般向“单个资源”型uri发起

PUT/animals/1

PUT /zoos/1

DELETE：删除

DELETE/zoos/1/employees/2

DELETE/zoos/1/employees/2;4;5

DELETE/zoos/1/animals //删除id为1的动物园内的所有动物

HEAD / OPTION/ PATCH用的不多，就不多解释了。

HEAD：获取资源的元数据

OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的

PATCH：在服务器更新资源（客户端提供改变的属性）

**安全性和幂等性**

1. 安全性 ：不会改变资源状态，可以理解为只读的；

2. 幂等性 ：执行1次和执行N次，对资源状态改变的效果是等价的。

.

安全性

幂等性

GET

√

√

POST

×

×

PUT

×

√

DELETE

×

√

安全性和幂等性均不保证反复请求能拿到相同的response。以 DELETE为例，第一次DELETE返回200表示删除成功，第二次返回404提示资源不存在，这是允许的。

**复杂查询**

查询可以捎带以下参数：

.

示例

备注

过滤条件

?type=1&age=16

允许一定的uri冗余，如 /zoos/1 与 /zoos?id=1

排序

?sort=age&order=asc

指定返回结果按照哪个属性排序，以及排序顺序

投影

?whitelist=id,name,email

分页

? page=2&per\_page=100

指定第几页，以及每页的记录数

**Bookmarker**

经常使用的、复杂的查询标签化，降低维护成本。

如：GET /trades?status=closed&sort=created,desc

快捷方式：GET /trades\#recently-closed或者GET /trades/recently-closed

**状态码**

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

§200 OK - \[GET\]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。

§201 CREATED - \[POST/PUT/PATCH\]：用户新建或修改数据成功。

§202 Accepted - \[\*\]：表示一个请求已经进入后台排队（异步任务）

§204 NO CONTENT - \[DELETE\]：用户删除数据成功。

§400 INVALID REQUEST - \[POST/PUT/PATCH\]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。

§401 Unauthorized - \[\*\]：表示用户没有权限（令牌、用户名、密码错误）。

§403 Forbidden - \[\*\] 表示用户得到授权（与401错误相对），但是访问是被禁止的。

§404 NOT FOUND - \[\*\]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。

§406 Not Acceptable - \[GET\]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。

§410 Gone -\[GET\]：用户请求的资源被永久删除，且不会再得到的。

§422 Unprocesable entity - \[POST/PUT/PATCH\] 当创建一个对象时，发生一个验证错误。

§500 INTERNAL SERVER ERROR - \[\*\]：服务器发生错误，用户将无法判断发出的请求是否成功。

状态码的完全列表参见这里

**URI失效**

随着系统发展，总有一些API失效或者迁移，对失效的API，返回404 not found 或 410 gone；对迁移的API，返回 301重定向。

四、**Response**

1. 不要包装：

response的 body 直接就是数据，不要做多余的包装。错误示例：

**\{**

 **"success":true,**

 **"data":\{"id":1,"name":"xiaotuan"\},**

**\}**

2. 各HTTP方法成功处理后的数据格式：

**·**

**response 格式**

GET

单个对象、集合

POST

新增成功的对象

PUT/PATCH

更新成功的对象

DELETE

空

**五、错误处理**

1. 不要发生了错误但给2xx响应，客户端可能会缓存成功的http请求；

2. 正确设置http状态码，不要自定义；

3. Response body提供

即:返回的信息中将error作为键名，出错信息作为键值即可

1)错误的代码（日志/问题追查）；

2)错误的描述文本（展示给用户）。

对第三点的实现稍微多说一点：

Java服务器端一般用异常表示 RESTful API的错误。API 可能抛出两类异常：业务异常和非业务异常。 业务异常 由自己的业务代码抛出，表示一个用例的前置条件不满足、业务规则冲突等，比如参数校验不通过、权限校验失败。 非业务类异常 表示不在预期内的问题，通常由类库、框架抛出，或由于自己的代码逻辑错误导致，比如数据库连接失败、空指针异常、除0错误等等。

业务类异常必须提供2种信息：

1. 如果抛出该类异常，HTTP响应状态码应该设成什么；

2. 异常的文本描述；

在Controller层使用统一的异常拦截器：

1. 设置 HTTP响应状态码：对业务类异常，用它指定的 HTTPcode；对非业务类异常，统一500；

2. Response Body的错误码：异常类名

3. Response Body的错误描述：对业务类异常，用它指定的错误文本；对非业务类异常，线上可以统一文案如“服务器端错误，请稍后再试”，开发或测试环境中用异常的 stacktrace，服务器端提供该行为的开关。

常用的http状态码及使用场景：

状态码

使用场景

400 bad request

常用在参数校验

401 unauthorized

未经验证的用户，常见于未登录。如果经过验证后依然没权限，应该 403（即 authentication和 authorization的区别）。

403 forbidden

无权限

404 not found

资源不存在

500 internal server error

非业务类异常

503 service unavaliable

由容器抛出，自己的代码不要抛这个异常

**六、其他**

（1）API的身份认证应该使用OAuth2.0框架

（2）服务器返回的数据格式，应该尽量使用JSON，避免使用XML

（3）比较复杂的接口不能确定是使用POST还是PUT时，要看具体的业务层代码，看看接口产生的结果是否幂等，如果幂等用PUT，相反用POST

如：接口接收到一资源，资源存在更新，不存在插入新数据，这个接口就要用PUT

![关于RESTful一些注意事项和自己整理的接口开发规范][RESTful]


[RESTful]: /pro/os/crawler/UFNM-ZUNZ-Q3EZ.jpg
 *  **原文作者：** 程序员小新人学习
 *  **原文链接：** https://www.toutiao.com/item/6571314650686685710/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。