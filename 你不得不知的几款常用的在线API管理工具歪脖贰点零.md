---
title: 你不得不知的几款常用的在线API管理工具
date: 2017-10-23 06:26:39
categories: "开发"
tags:
	- Rap
	- Java
	- 可视化
	- 软件
	- HTML

---

在项目开发过程中，总会涉及到接口文档的设计编写，之前使用的都是ms office工具，不够漂亮也不直观，变更频繁的话维护成本也更高，及时性也是大问题。基于这个背景，下面介绍几个常用的API管理工具，方便你与调用方更高效的沟通测试：

Swagger

官网地址：https://swagger.io Swagger 是一款RESTFUL接口的文档在线自动生成+功能测试功能软件，是一个规范和完整的框架，标准的，语言无关，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。总体目标是使客户端和文件系统作为服务器以同样的速度来更新。文件的方法，参数和模型紧密集成到服务器端的代码，允许API来始终保持同步。Swagger 让部署管理和使用功能强大的API从未如此简单。

目前最新版本是V3，SwaggerUI是一个简单的Restful API 测试和文档工具。简单、漂亮、易用。通过读取JSON 配置显示API. 项目本身仅仅也只依赖一些 html,css.js静态文件. 你可以几乎放在任何Web容器上使用。

![你不得不知的几款常用的在线API管理工具][API]

RAP

官网地址：http://rapapi.org/org/index.do

RAP来自阿里巴巴，是一个可视化接口管理工具 通过分析接口结构，使用mock动态生成模拟数据，校验真实接口正确性， 围绕接口定义，通过一系列自动化工具提升我们的协作效率。可以在线使用，也可以选择本地部署。一个GUI的WEB接口管理工具。在RAP中，您可定义接口的URL、请求&响应细节格式等等。通过分析这些数据，RAP提供MOCK服务、测试服务等自动化工具。RAP同时提供大量企业级功能，帮助企业和团队高效的工作。

在前后端分离的开发模式下，我们通常需要定义一份接口文档来规范接口的具体信息。如一个请求的地址、有几个参数、参数名称及类型含义等等。RAP 首先方便团队录入、查看和管理这些接口文档，并通过分析结构化的文档数据，重复利用并生成自测数据、提供自测控制台等等... 大幅度提升开发效率。

![你不得不知的几款常用的在线API管理工具][API 1]

APIDOC

GitHub 地址：https://github.com/apidoc/apidoc

APIDOC可以根据代码注释生成WEB API文档，支持大部分主流开发语言，Java、javascript、php、erlang、perl、python、ruby等等，相对而言，web接口的注释维护起来更加方便，不需要额外再维护一份文档。APIDOC从注释生成静态html网页文档，不仅支持项目版本号，还支持API版本号。

操作步骤也是相当简单，依据官网的操作指南完成一个简单的示例。这是一个示例demo，感受一下http://apidocjs.com/example\_basic/

![你不得不知的几款常用的在线API管理工具][API 2]

Spring REST Docs

官网地址：http://projects.spring.io/spring-restdocs/

Spring的文档帮助产生RESTful的服务文档。它结合了手写文档写的asciidoctor和自动生成与Spring MVC测试生成的片段。这种方法可以让你突破Swagger那样的工具产生的文件的局限性。它可以帮助你制作文件，准确，简洁，结构良好。生成的文档，可以让你的用户得到一个最低限度的他们所需要的信息。

![你不得不知的几款常用的在线API管理工具][API 3]

其它

除了上面介绍到一些开源或免费的API管理工具，国内外同样也有一些公司在做这个事情，根据使用需求做好选型即可，适合自己的才是最好的。


[API]: /pro/os/crawler/AJIJ-UE6Z-QQZM.jpg
[API 1]: /pro/os/crawler/VZFE-UQRN-B7RR.jpg
[API 2]: /pro/os/crawler/2QMU-RFBJ-7NVN.jpg
[API 3]: /pro/os/crawler/EEVY-2Y6V-FYYY.jpg
 *  **原文作者：** 歪脖贰点零
 *  **原文链接：** https://www.toutiao.com/item/6479865260386812430/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。