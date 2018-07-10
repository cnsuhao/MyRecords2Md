---
title: 使用基于令牌JWT验证SPA来保护您的单页应用程序
date: 2018-04-23 10:41:42
categories: "开发"
tags:
	- 技术
	- 路由器
	- JSON
	- Node.js

---

成功的令牌认证系统要求您了解安全细节和其他认证凭证。SPA'通常绑定到Restful API以将您的UI与所需数据绑定在一起，并为您的UI带来更好的外观和感觉。但问题是，你如何以安全的方式打开你的API访问？

一个这样的SPA框架是Angular。我们可以借助基于JSON Web令牌的身份验证来验证我们的Angular应用程序。因此，当您创建Angular应用程序时，一个大问题是，您如何知道您的用户是谁以及他们可以访问哪些内容？

![使用基于令牌JWT验证SPA来保护您的单页应用程序][JWT_SPA]

针对所有这些问题的答案以及针对API的认证和授权问题的解决方案是JWT认证系统，它正在取代传统的基于cookie的认证。这给你一个很好的方式来声明一个用户及其在应用程序中的访问权限。它为您提供了一个加密标头作为令牌，以保护未经身份验证的用户的应用程序安全，并为您的前端和后端构建访问控制逻辑。

让我们通过在您的Angular应用程序（版本2+）中查看几个验证代码来深入了解它。

![使用基于令牌JWT验证SPA来保护您的单页应用程序][JWT_SPA 1]

# **前端工作** #

当用户发出登录请求时，我们通过我们的身份验证服务调用后端，如下所示：

login(username: string, password: string): Observable < boolean > \{

return this.http.post('/api/authenticate', JSON.stringify(\{

username: username,

password: password

\}))

.map((response: Response) => \{

// 如果响应中有一个jwt令牌，则登录成功

lettoken = response.json() && response.json().token;

if (token) \{

// 设置令牌属性

this.token = token;

// 在本地存储中存储用户名和jwt令牌以保持用户在页面刷新之间登录

localStorage.setItem('currentUser', JSON.stringify(\{

username: username,

token: token

\}));

// 返回true表示成功登录

returntrue;

\} else \{

// 返回false表示登录失败

returnfalse;

\}

\});

\}

![使用基于令牌JWT验证SPA来保护您的单页应用程序][JWT_SPA 2]

# **服务器端工作** #

当用户尝试使用用户名和密码登录应用程序时，服务器将：

 *  验证用户。
 *  生成令牌。
 *  将令牌发送给用户。

后端可以是像Node.js这样的任何服务器端框架，它使用npm'jsonwetoken'模块来签署和验证JSON Web令牌。以下是一些示例代码：

模块：

var jwt = require（“jsonwebtoken”）;

var bcrypt = require（“bcryptjs”）;

和登录功能：

login: function (req, res) \{

// 这是参数检查是否提供

if (!\_.has(req.body, 'email') || !\_.has(req.body, 'password')) \{

returnres.serverError("No field should be empty.");

\}

// 检查用户名是否与任何电子邮件或电话号码匹配

User.findOne(\{

email: req.body.email

\}).exec(functioncallback(err, user) \{

if (err) returnres.serverError(err);

if (!user) returnres.serverError("User not found, please sign up.");

//检查密码

bcrypt.compare(req.body.password, user.password, function (error, matched) \{

if (error) returnres.serverError(error);

if (!matched) returnres.serverError("Invalid password.");

//保存生成令牌的日期

vartoken = jwt.sign(user.toJSON(), "this is my secret key", \{

expiresIn: '10m'

\});

//返回令牌

res.ok(token);

\});

\});

\}

# **令牌返回如何工作？** #

用户使用其凭据成功登录后，会返回存储在本地存储中的Web令牌。现在，无论何时用户想要访问受保护的路由，它都应该使用承载架构在授权标头中发送JWT。标题的内容如下所示：

Authorization: Bearer

因此，JWT将被用于认证和授权API，这些API将授予访问其受保护的路由和资源的权限。

![使用基于令牌JWT验证SPA来保护您的单页应用程序][JWT_SPA 3]

# **使用Angular处理Web令牌**  #

由于JWT现在存储在本地存储中，我们可以使用它来防止未经身份验证的用户访问受限制的路由。它用于路由模块以保护任何路由。为此，我们 AuthGuard 在Angular 2中 AuthGuard 使用该 CanActivate 方法 @angular/router。

import \{ Injectable \} from'@angular/core';

import \{ Router, CanActivate \} from'@angular/router';

@Injectable()

exportclassAuthGuardimplementsCanActivate \{

constructor(privaterouter: Router) \{ \}

canActivate() \{

if (localStorage.getItem('currentUser')) \{

// 登录后返回True

returntrue;

\}

// 未登录，重定向登录页面

this.router.navigate(\['/login'\]);

returnfalse;

\}

\}

因此，可以像这样在路由模块中保护路由：

\{ path: '', component: SomeComponent, canActivate: \[AuthGuard\] \}

接下来，我们使用此令牌来获取我们SPA所需的数据。它可以访问用户的API端点。此端点将以SPA为用户需要的数据作出响应。我们可以使用用户服务，如下所示。

getUsers(): Observable<User\[\]> \{

// 使用JWT令牌添加授权标头

let headers = new Headers(\{ 'Authorization': 'Bearer ' + this.authenticationService.token \});

letoptions = new RequestOptions(\{ headers: headers \});

// 从API获取用户

returnthis.http.get('/api/users', options)

.map((response: Response) => response.json());

\}

注意我们如何使用RequestOptions 上面讨论的Authorization Bearer 附加标题 。为此，可以使用npm中的另一个模块，即'angular2-jwt'。它是与Angular 2应用程序中的JWT合作的帮助程序库。当从您的应用程序发出HTTP请求时，它会自动将JWT作为授权标头附加。它使用它的AuthHttp 类发送一个JWT请求 （作为提供程序中的一个工厂函数在app.module.ts中导入），从而有条件地允许基于JWT状态的路线导航。它可以按照如下所示进行安装。

npm install angular2-jwt

AuthHTTP 可以在调用用户API时使用，如下所示：

// 从API获取用户

returnthis.authHttp.get('/api/users')

.map((response: Response) => response.json());


[JWT_SPA]: http://p1.pstatp.com/large/pgc-image/1524451215768055ee04955
[JWT_SPA 1]: http://p3.pstatp.com/large/pgc-image/152445123666995f50535ea
[JWT_SPA 2]: http://p3.pstatp.com/large/pgc-image/15244512642439a51da1291
[JWT_SPA 3]: http://p1.pstatp.com/large/pgc-image/15244512846828b5dcced7c
 *  **原文作者：** 空空教你玩黑客
 *  **原文链接：** https://www.toutiao.com/item/6547468490142384654/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。