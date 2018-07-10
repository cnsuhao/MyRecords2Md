---
title: 「系统架构」API Gateway——API管理的正确姿势
date: 2018-06-22 07:00:00
categories: "开发"
tags:
	- 技术

---

# 引言 #

随着微服务的大红大紫，大家纷纷使用微服务架构来实现新系统或进行老系统的改造。当然，微服务带给我们太多的好处，同时也带给我们许多的问题需要解决。采用微服务后，所有的服务都变成了一个个细小的API，那么这些服务API该怎么正确的管理？API认证授权如何实现？如何实现服务的负载均衡，熔断，灰度发布，限流流控？如何合理的治理这些API服务尤其重要。在微服务架构中，API Gateway作为整体架构的重要组件，它抽象了微服务中都需要的公共功能，同时提供了客户端负载均衡，服务自动熔断，灰度发布，统一认证，限流流控，日志统计等丰富的功能，帮助我们解决很多API管理难题。

# 什么是API Gateway #

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API]

（图片来自网络）

这张图，非常形象的解释了API Gateway和微服务的关系。做为听众，我们只想听到美妙动人的音乐旋律，完全不会在乎音乐是怎么演奏的。而指挥家则根据曲谱负责指挥好每一个演奏者，使每一个演奏者之间配合默契，共同完成这场音乐演出。在微服务的世界里，API Gateway就如同一位指挥家，“指挥”着不同的“演奏人”(微服务)。

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 1]

我们知道在微服务架构中，大型服务都被拆分成了独立的微服务，每个微服务通常会以RESTFUL API的形式对外提供服务。但是在UI方面，我们可能需要在一个页面上显示来自不同微服务的数据，此时就会需要一个统一的入口来进行API的调用。上图中我们可以看到，API Gateway就在此场景下充当了多个服务的大门，系统的统一入口，从面向对象设计的角度看，它与外观模式类似，API Gateway封装了系统的内部复杂结构，同时它还可能具有其他API管理/调用的通用功能，如认证，限流，流控等功能。

# 为什么需要API Gateway #

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 2]

首先，Chris Richardson在http://microservices.io/中也提及到，在微服务的架构模式下，API Gateway是微服务架构中一个非常通用的模式，利用API Gateway可以解决调用方如何调用独立的微服务这个问题。

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 3]

从部署结构上说，上图是不采用API Gateway的微服务部署模式，我们可以清晰看到，这种部署模式下，客户端与负载均衡器直接交互，完成服务的调用。但这是这种模式下，也有它的不足。

 *  不支持动态扩展，系统每多一个服务，就需要部署或修改负载均衡器。
 *  无法做到动态的开关服务，若要下线某个服务，需要运维人员将服务地址从负载均衡器中移除。
 *  对于API的限流，安全等控制，需要每个微服务去自己实现，增加了微服务的复杂性，同时也违反了微服务设计的单一职责原则。

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 4]

上图为采用API Gateway模式，我们通过上图可以看到，API Gateway做为系统统一入口，实现了对各个微服务间的整合，同时又做到了对客户端友好，屏蔽系统了复杂性和差异性。对比之前无API Gateway模式，API Gateway具有几个比较重要的优点：

 *  采用API Gateway可以与微服务注册中心连接，实现微服务无感知动态扩容。
 *  API Gateway对于无法访问的服务，可以做到自动熔断，无需人工参与。
 *  API Gateway可以方便的实现蓝绿部署，金丝雀发布或A/B发布。
 *  API Gateway做为系统统一入口，我们可以将各个微服务公共功能放在API Gateway中实现，以尽可能减少各服务的职责。
 *  帮助我们实现客户端的负载均衡策略。

# API Gateway中一些重要的功能 #

下面我们用图来说明API Gateway中一些重要的功能：

**负载均衡**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 5]

在实际的部署应用中，当应用系统面临大量访问，负载过高时，通常我们会增加服务数量来进行横向扩展，使用集群来提高系统的处理能力。此时多个服务通过某种负载算法分摊了系统的压力，我们将这种多节点分摊压力的行为称为负载均衡。

API Gateway可以帮助我们轻松的实现负载均衡，利用服务发现知道所有Service的地址和位置，通过在API Gateway中实现负载均衡算法，就可以实现负载均衡效果。

**服务熔断**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 6]

在实际生产中，一些服务很有可能因为某些原因发生故障，如果此时不采取一些手段，会导致整个系统“雪崩”。或因系统整体负载的考虑，会对服务访问次数进行限制。服务熔断、服务降级就是解决上述问题的主要方式。API Gateway可以帮助我们实现这些功能，对于服务的调用次数的限制，当某服务达到上限时，API Gateway会自动停止向上游服务发送请求，并像客户端返回错误提示信息或一个统一的响应，进行服务降级。对于需要临时发生故障的服务，API Gateway自动可以打开对应服务的断路器，进行服务熔断，防止整个系统“雪崩”。

**灰度发布**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 7]

服务发布上线过程中，我们不可能将新版本全部部署在生产环节中，因为新版本并没有接受真实用户、真实数据、真实环境的考验，此时我们需要进行灰度发布，灰度发布可以保证整体系统的稳定，在初始灰度的时候就可以发现、调整问题，同时影响小。API Gateway可以帮助我们轻松的完成灰度发布，只需要在API Gateway中配置我们需要的规则，按版本，按IP段等，API Gateway会自动为我们完成实际的请求分流。

# API Gateway vs 反向代理 #

**反向代理**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 8]

在传统部署架构中，反向代理，大多是用于多个系统模块间的聚合，实现负载均衡，外网向内网的转发。通过修改配置文件的方式来进行增加或删除节点，并重启服务才可生效。通常来说，反向代理服务器只具备负载均衡、转发基本功能，若要需要其他功能，需要增加扩展或提供脚本来实现。

**API Gateway**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 9]

在API Gateway部署模式中，API Gateway可以看作特殊的反向代理，是对反向代理服务器功能的扩充，同时API Gateway仅局限于服务API层面，对API做进一步的管理，保护。API Gateway不仅提供了负载均衡，转发功能，还提供了灰度发布，统一认证，熔断，消息转换，访问日志等丰富的功能。

如何选择？

倘若我们实际运用中，不需要服务发现，服务动态扩容，服务熔断，统一认证，消息转换等一系列API Gateway功能，我们完全可以使用反向代理服务器来部署微服务架构，当然如果这样做，如遇到增加或减少服务节点时，需要修改反向代理服务器配置，重启服务才可以生效。而当我们可能不仅仅需要负载均衡，内外网转发，还需要其他功能，又同时想实现一些各服务都需要的通用的功能时，这时候就改考虑API Gateway了。

# API Gateway对API的认证及鉴权 #

目前在微服务中，我们还需要考虑如何保护我们的API只能被同意授权的客户调用。那么对于API的保护，目前大多数采用的方式有这么几种，分别为AppKeys，OAuth2 和 OAuth2+JWT，接下来我们逐个了解下。

**认证方式**

1）AppKeys

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 10]

目前采用AppKeys Auth认证的公有云API Gateway和数据开放平台居多，如阿里API Gateway，聚合数据等，这种认证模式是由API Gateway颁发一个key，或者appkey+appsecret+某种复杂的加密算法生成AppKey，调用方获取到key后直接调用API。这个key可以是无任何意义的一串字符。API Gateway在收到调用API请求时，首先校验key的合法性，包括key是否失效，当前调用API是否被订阅等等信息，若校验成功，则请求上游服务，返回结果。此处上游服务不再对请求做任何校验，直接返回结果。采用AppKeys认证模式比较适合Open Service的场景。其中并不涉及到用户信息，权限信息。

2）OAuth2

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 11]

大部分场景中，我们还是需要有知道谁在调用，调用者是否有对应的角色权限等。OAuth2可以帮助我们来完成这个工作。在OAuth2的世界中，分为以下几种角色：Resource Owner，Client，Authorization Server，Resource Sever。上图是一个简单的OAuh2流程来说明各个角色之前的关系。最终，App获得了Rory的个人信息。

3）OAuth2+JWT

OAuth2 + JWT流程跟OAuth2完全一致。了解OAuth2的童鞋都应该知道，OAuth2最后会给调用方颁发一个Access Token，OAuth2+JWT实际上就是将Access Token换成JWT而已。这样做的好处仅仅是减少Token校验时查询DB的次数。

**OAuth2 with API Gateway**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 12]

有那么多认证方式，加入了API Gateway后，该怎么做呢？在微服务的世界中，我们每个服务可能都会需要用户信息，用户权限来判断当前接口或功能是否对当前用户可用。

我们可以将统一认证放在API Gateway来实现，由API Gateway来做统一的拦截和鉴权，结合上文所描述的认证方式中，OAuth2协议中可以携带用户信息，故采用OAuth2。

# 采用OAuth2方式认证的两种部署方式 #

**VPC网络部署，服务内部授信**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 13]

第一种，微服务部署在单独的VPC网络中，同时微服务互相授信，互相调用不需要验证请求是否合法。这种模式下，当调用请求经过API Gateway时，API Gateway会拿着调用者提供的Access Token到Authorization Server中认证，若Access Token合法，Authorization Server会返回当前Access Token所代表的基本信息，API Gateway获取到这些基本信息后，会将这些信息放置在自定义Header中请求上游服务，上游服务可获取这些基本信息来进行对应操作的权限判断。如果我们对API Gateway跟Authorization Server验证Access token的过程中，担心有性能和效率损失，我们可以将Access Token改为JWT token，由API Gateway持有公钥对JWT token进行解密，减少一次HTTP请求。但是这种做法不推荐，毕竟JWT基本信息是Base64的，可以被轻而易举的解密。

**微服务互相不授信，不在VPC中**

![「系统架构」API Gateway——API管理的正确姿势][API Gateway_API 14]

第二种，微服务互相不授信，彼此调用需要验证请求的合法性，这种模式为了更加安全，我们需要内外token转换。首先，调用方通过OAuth2流程，获取到Access Token，当前Access Token是一串没有意义的字符串，我们将它称为Reference Token。当调用方调用API时，此时API Gateway会拿着调用者提供的Access Token到Authorization Server中认证置换。此时，Authorization Server不会返回基本信息，而是返回一个包含基本信息的JWT Token，我们将它称为ID Token。紧接着API Gateway会将JWT Token放到Request Header中，请求上游服务。上游服务获取到当前JWT token后，会请求Authorization Server获取到JWK，利用JWK对当前JWT Token进行校验，解密，最后，进行角色权限判断。微服务之间的互相调用，也必须将JWT Token放在Request Header中。

总结来说，以上几种方式都可以完成认证，但具体采用什么方式认证，还是取决于实际中对系统安全性的要求和具体的业务场景。

# 总结 #

API Gateway在微服务架构中起到了至关重要的作用。在文章中我们介绍了什么是API Gateway以及为什么需要API Gateway。API Gateway它作为微服务系统的大门，向我们提供了请求转发，服务熔断，限流流控等公共功能，它又统一整合了各个微服务，对外屏蔽了系统的复杂性和差异性。

另外，我们介绍了目前微服务架构中几种认证方案。结合API Gateway，我们采用了OAuth2协议对API进行认证鉴权，同时又从在部署架构的角度上，介绍了两种不一样的认证方式。

--------------------

本文转载自“微信公众号EAWorld”，由“编码之道”编辑整理。


[API Gateway_API]: /pro/os/crawler/3UYQ-FUMR-RNY3.jpg
[API Gateway_API 1]: /pro/os/crawler/F7FM-2Q7R-2U3A.jpg
[API Gateway_API 2]: /pro/os/crawler/3UBE-QFII-EIEQ.jpg
[API Gateway_API 3]: /pro/os/crawler/ZZUJ-YIIM-ME22.jpg
[API Gateway_API 4]: /pro/os/crawler/FBQV-3IEA-ZAJB.jpg
[API Gateway_API 5]: /pro/os/crawler/BNYF-VEFQ-ZINF.jpg
[API Gateway_API 6]: /pro/os/crawler/IZ6V-2UUA-3MNJ.jpg
[API Gateway_API 7]: /pro/os/crawler/3AUI-7NBJ-NYNA.jpg
[API Gateway_API 8]: /pro/os/crawler/FIMZ-RIYZ-J6V3.jpg
[API Gateway_API 9]: /pro/os/crawler/2YZB-ZABM-ABJ2.jpg
[API Gateway_API 10]: /pro/os/crawler/ZV2E-BBNE-NYEF.jpg
[API Gateway_API 11]: /pro/os/crawler/VVZN-QFQY-ZAAF.jpg
[API Gateway_API 12]: /pro/os/crawler/MEBE-IUZN-FVJU.jpg
[API Gateway_API 13]: /pro/os/crawler/ENRA-BUNM-N3EV.jpg
[API Gateway_API 14]: /pro/os/crawler/MNYQ-M32Q-UJBV.jpg
 *  **原文作者：** 编码之道
 *  **原文链接：** https://www.toutiao.com/item/6569529050505675277/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。