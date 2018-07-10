---
title: Swagger-API文档生成框架的基本使用
date: 2018-05-23 23:39:00
categories: "开发"
tags:
	- Java
	- CSS
	- GitHub
	- 编程语言
	- JSON

---

> 使用工具、框架的目的就是为了开发过程简洁方便或者是达到更强大的功能效果。在目前互联网开发发展的趋势下，由于技术更加深度化、细化，前后端分离开发成为必不可少的一个环节，而后端和前端开发人员之间唯一的桥梁便是API文档，也就是接口�����档。 平时开发中大部分接口开发都会由开发者或者特定人员手写接口文档。而文档也会随着项目迭代不停更新，由此需要不停有人员维护API文档的内容。消耗时间同时也需要消耗人力资源。由此原因我有了接触和了解一些API文档生成工具的机会。 了解到了部分的API工具，首先的话是RAP，这个项目好像是阿里开发的，github地址为：https://github.com/thx/RAP 这里的话最后还是选���了swagger，以下先介绍下swagger这个工具

**1.swagger**

1.1 官网

> https://swagger.io/

![Swagger-API文档生成框架的基本使用][Swagger-API]

``````````
1.2 简述
``````````

> Swagger项目是由Dilip Krishnan和Adrian Kelly等人维护开发的一个为Spring Web MVC 项目提供方法文档的一个框架。该框架最主要的功能是将Controller的方法进行可视化的展现，像方法注释，方法参数，方法返回值等都提供了相应的用户界面，尤其是对JSON参数的支持。同时可以结合swagger-ui可以对用户界面进行不同程度的定制，也可以对方法进行一个简单的测试。
> 
> Swagger API框架，用于管理项目中API接口，属当前最流行的API接口管理工具。 Swagger是一个开源框架(Web框架)，功能强大，UI界面漂亮，支持在线测试！
> 
> Swagger的目标是对REST API定义一个标准的和语言无关的接口，可让人和计算机无需访问源码、文档或���络���量监测就可以发现和理解服务的能力。当通过Swagger进行正确定义，用户可以理解远程服务并使用最少实现逻辑与远程服务进行交互。与为底层编程所实现的接口类似，Swagger消除了调用服务时可能会有的猜测。
> 
> Swagger是一组开源项目，主要项目如下：
> 
> Swagger-tools:提供各种与Swagger进行集成和交互的工具。例如模式检验、Swagger 1.2文档转换成Swagger 2.0文档等功能。
> 
> Swagger-core: 用于Java/Scala的的Swagger实现。与JAX-RS(Jersey、Resteasy、CXF…)、Servlets和Play框架进行集成。
> 
> Swagger-js: 用于JavaScript的Swagger实现。
> 
> Swagger-node-express: Swagger模块，用于node.js的Express web应用框架。
> 
> Swagger-ui：一个无依赖的HTML、JS和CSS集合，可以为Swagger兼容API动态生成优雅文档。
> 
> Swagger-codegen：一个模板驱动引擎，通过分析用户Swagger资源声明以各种语言生成客户���代码。
> 
> Swagger-editor：可让使用者在浏览器里以YAML格式编辑Swagger API规范并实时预览文档。可以生成有效的Swagger JSON描述，并用于所有Swagger工具（代码生成、文档等等）中。

**2.swagger使用**

``````````
在尝试使用swagger的过程中碰到了一些问题，看到了一些网友博客都讲的很不错，在下面会列出来以供大家一起学习。在各种博客中，spring-web项目集成swagger主要看到的有两种方��，这里��介绍我目前使用的这种
``````````

**2.1 第一种方式:**

``````````
2.1.1 首先构建一个spring-web项目2.1.2 pom引用
``````````

**pom.xml**

``````````
<dependency> <groupId>io.springfox</groupId> <artifactId>springfox-swagger2</artifactId> <version>2.7.0</version></dependency><dependency> <groupId>com.fasterxml.jackson.core</groupId> <artifactId>jackson-databind</artifactId> <version>2.6.5</version></dependency>2.1.3 SpringfoxConfig 配置swagger
``````````

**SpringfoxConfig**

``````````
import org.springframework.context.annotation.Bean;import org.springframework.context.annotation.ComponentScan;import org.springframework.context.annotation.Configuration;import springfox.documentation.builders.ApiInfoBuilder;import springfox.documentation.builders.PathSelectors;import springfox.documentation.builders.RequestHandlerSelectors;import springfox.documentation.service.ApiInfo;import springfox.documentation.service.Contact;import springfox.documentation.spi.DocumentationType;import springfox.documentation.spring.web.plugins.Docket;import springfox.documentation.swagger2.annotations.EnableSwagger2;/** * Swagger2 Config * * @author hzk */@Configuration@EnableSwagger2@ComponentScan("com.ithzk.swagger.controller")public class SpringfoxConfig { @Bean public Docket createRestApi() { return new Docket(DocumentationType.SWAGGER_2) .apiInfo(apiInfo()) .select() .apis(RequestHandlerSelectors.basePackage("com.ithzk.swagger.controller")) .paths(PathSelectors.any()) .build(); } private ApiInfo apiInfo() { Contact contact = new Contact("HuZeKun", "", "huzekunk@foxmail.com"); return new ApiInfoBuilder() .title("Swagger 1.x API接口文档") .description("") .contact(contact) .version("1.0.0") .build(); }}2.1.4 接下来就可以编写接口并且加上相应注解
``````````

**BusinessController**

``````````
import com.ithzk.swagger.entity.dto.GoodsDto;import com.ithzk.swagger.entity.req.GoodsReq;import io.swagger.annotations.Api;import io.swagger.annotations.ApiOperation;import io.swagger.annotations.ApiParam;import org.springframework.http.MediaType;import org.springframework.stereotype.Controller;import org.springframework.validation.BindingResult;import org.springframework.web.bind.annotation.RequestMapping;import org.springframework.web.bind.annotation.RequestMethod;import javax.validation.Valid;import java.io.PrintWriter;/** * 业务接口 * @author hzk * @date 2018/4/19 */@Controller@RequestMapping("api/business/")@Api(value="商业化接口",tags = "接口信息",description = "商业相关接口")public class BusinessController { @RequestMapping(value = "goods_info", method = RequestMethod.GET, produces = MediaType.APPLICATION_JSON_VALUE) @ApiOperation(value = "获取商品信息", httpMethod = "GET",response = GoodsDto.class) public GoodsDto getDeskTopInfo(PrintWriter out, @Valid @ApiParam(value = "params", required = true) GoodsReq req, BindingResult bindingResult) throws Exception { GoodsDto goodsDto = new GoodsDto(); if (bindingResult.hasErrors()) { goodsDto.setGoodsId(2); goodsDto.setGoodsName("上好佳"); } else { goodsDto.setGoodsId(1); goodsDto.setGoodsName("浪味仙"); } return goodsDto; }}
``````````

**BaseInfoRequest**

``````````
import io.swagger.annotations.ApiParam;import javax.validation.constraints.NotNull;import javax.validation.constraints.Pattern;public class BaseInfoRequest { @ApiParam(name = "version", value = "接口版本号", defaultValue = "1.0", required = true, allowableValues = "1.0") @Pattern(regexp = "1.0", message = "version filed must equal 1.0") protected String version; @ApiParam(name = "format", value = "接口响应格式[json,jsonp]", required = true, defaultValue = "json", allowableValues = "json,jsonp") @NotNull protected String format; public String getVersion() { return version; } public void setVersion(String version) { this.version = version; } public String getFormat() { return format; } public void setFormat(String format) { this.format = format; } @Override public String toString() { return "BaseInfoRequest{" + "version='" + version + ''' + ", format='" + format + ''' + '}'; }}
``````````

**GoodsReq**

``````````
import com.ithzk.entity.req.base.BaseInfoRequest;import io.swagger.annotations.ApiParam;import javax.validation.constraints.NotNull;/** * 商品请求相关 * @author hzk * @date 2018/4/19 */public class GoodsReq extends BaseInfoRequest { @ApiParam(name = "goodsType", value = "商品类别", required = true) @NotNull private Integer goodsType; public Integer getGoodsType() { return goodsType; } public void setGoodsType(Integer goodsType) { this.goodsType = goodsType; } @Override public String toString() { return "GoodsReq{" + "goodsType=" + goodsType + '}'; }}
``````````

**GoodsDto**

``````````
import com.alibaba.fastjson.annotation.JSONType;import io.swagger.annotations.ApiModel;import io.swagger.annotations.ApiModelProperty;/** * 商品相关 * @author hzk * @date 2018/4/19 */@JSONType(orders={"goodsId","goodsName"})@ApiModel(value = "goods",description = "商品信息")public class GoodsDto { @ApiModelProperty(value = "商品ID",name = "goods_id") private Integer goodsId; @ApiModelProperty(value = "商品名称",name = "goods_name") private String goodsName; public Integer getGoodsId() { return goodsId; } public void setGoodsId(Integer goodsId) { this.goodsId = goodsId; } public String getGoodsName() { return goodsName; } public void setGoodsName(String goodsName) { this.goodsName = goodsName; } @Override public String toString() { return "GoodsDto{" + "goodsId=" + goodsId + ", goodsName='" + goodsName + ''' + '}'; }}2.1.5 此时启动项目，访问项目basePath/v2/api-docs会展现以下格式生成的数据则成功
``````````

![Swagger-API文档生成框架的基本使用][Swagger-API 1]

2.1.6 引入swagger-ui

**pom.xml**

> <dependency>
> 
> <groupId>io.springfox</groupId>
> 
> <artifactId>springfox-swagger-ui</artifactId>
> 
> <version>2.8.0</version>
> 
> </dependency>

2.1.7 启动��目访问basePath/swagger-ui.html即可

在这一步，我本地出现以下问题，尝试了几种解决方式，但是暂时未解决，以后解决了会补上解决方式

![Swagger-API文档生成框架的基本使用][Swagger-API 2]

在此暂时通过其他服务器部署好的swagger-ui来访问当前项目生成的数据

![Swagger-API文档生成框架的基本使用][Swagger-API 3]

![Swagger-API文档生成框架的基本使用][Swagger-API 4]

![Swagger-API文档生成框架的��本使用][Swagger-API 5]

通过点击图上Try it out可以访问并测试接口功能，至此就完成了swagger的嵌入

**2.2 第二种方式**再介绍一下第二种方式

2.2.1 同理构建一个spring-web项目

2.2.2 pom引入

**pom.xml**

> <dependency>
> 
> <groupId>com.mangofactory</groupId>
> 
> <artifactId>swagger-springmvc</artifactId>
> 
> <version>1.0.2</version>
> 
> </dependency>
> 
> <dependency>
> 
> <groupId>com.mangofactory</groupId>
> 
> <artifactId>swagger-models</artifactId>
> 
> <version>1.0.2</version>
> 
> </dependency>
> 
> <dependency>
> 
> <groupId>com.wordnik</groupId>
> 
> <artifactId>swagger-annotations</artifactId>
> 
> <version>1.3.11</version>
> 
> </dependency>
> 
> <!-- swagger-springmvc dependencies -->
> 
> <dependency>
> 
> <groupId>com.google.guava</groupId>
> 
> <artifactId>guava</artifactId>
> 
> <version>15.0</version>
> 
> </dependency>
> 
> <dependency>
> 
> <groupId>com.fasterxml.jackson.core</groupId>
> 
> <artifactId>jackson-annotations</artifactId>
> 
> <version>2.4.4</version>
> 
> </dependency>
> 
> <dependency>
> 
> <groupId>com.fasterxml.jackson.core</groupId>
> 
> <artifactId>jackson-databind</artifactId>
> 
> <version>2.4.4</version>
> 
> </dependency>
> 
> <dependency>
> 
> <groupId>com.fasterxml.jackson.core</groupId>
> 
> <artifactId>jackson-core</artifactId>
> 
> <version>2.4.4</version>
> 
> </dependency>
> 
> <dependency>
> 
> <groupId>com.fasterxml</groupId>
> 
> <artifactId>classmate</artifactId>
> 
> <version>1.1.0</version>
> 
> </dependency>
> 
> 2.2.3 SwaggerConfig 配置swagger
> 
> **SwaggerConfig**package com.swagger.config;
> 
> import com.mangofactory.swagger.configuration.SpringSwaggerConfig;
> 
> import com.mangofactory.swagger.models.dto.ApiInfo;
> 
> import com.mangofactory.swagger.plugin.EnableSwagger;
> 
> import com.mangofactory.swagger.plugin.SwaggerSpringMvcPlugin;
> 
> import org.springframework.beans.factory.annotation.Autowired;
> 
> import org.springframework.context.annotation.Bean;
> 
> import org.springframework.context.annotation.ComponentScan;
> 
> import org.springframework.context.annotation.Configuration;
> 
> @Configuration
> 
> @EnableSwagger
> 
> @ComponentScan("com.swagger.controller") //启用组件扫描
> 
> public class SwaggerConfig \{
> 
> private SpringSwaggerConfig springSwaggerConfig;
> 
> /\*\*
> 
> \* Required to autowire SpringSwaggerConfig
> 
> \*/
> 
> @Autowired
> 
> public void setSpringSwaggerConfig(SpringSwaggerConfig springSwaggerConfig)
> 
> \{
> 
> this.springSwaggerConfig = springSwaggerConfig;
> 
> \}
> 
> /\*\*
> 
> \* Every SwaggerSpringMvcPlugin bean is picked up by the swagger-mvc
> 
> \* framework - allowing for multiple swagger groups i.e. same code base
> 
> \* multiple swagger resource listings.
> 
> \*/
> 
> @Bean
> 
> public SwaggerSpringMvcPlugin customImplementation()
> 
> \{
> 
> return new SwaggerSpringMvcPlugin(this.springSwaggerConfig)
> 
> .apiInfo(apiInfo())
> 
> .includePatterns(".\*?");
> 
> \}
> 
> private ApiInfo apiInfo()
> 
> \{
> 
> ApiInfo apiInfo = new ApiInfo(
> 
> "Swagger测试接口",
> 
> "这是一个Swagger生成API测试",
> 
> "My Apps API terms of service",
> 
> "huzekunk@foxmail.com",
> 
> "My Apps API Licence Type",
> 
> "My Apps API License URL");
> 
> return apiInfo;
> 
> \}
> 
> \}

2.2.4 其他Controller和Entity同理，构建完成启动即可

有些还不是很明白的是这种方式生成的地址是basUrl/api-docs,应该是对这个还不是很熟悉，之后���果有机会���入会继续记录分享给大家

![Swagger-API文档生成框架的基本使用][Swagger-API 6]

将该接口放至开始服务器无法读取接口信息

所以尝试使用原始方式搭建一个swagger-ui

2.2.5 spring.xml

> <mvc:resources mapping="/swagger/\*\*" location="/swagger/"/>
> 
> <!--未初始化此类会导致SwaggerConfig无法注入成功-->
> 
> <bean class="com.mangofactory.swagger.configuration.SpringSwaggerConfig" />
> 
> <bean class="com.swagger.config.SwaggerConfig"/>

2.2.6 搭建swagger-ui

> 下载 swagger-ui
> 
> https://github.com/swagger-api/swagger-ui/tree/v2.2.10
> 
> 解压下载后的文件，将里面dist文件夹下内容放置项目中

2.2.7 修改index.html内容

![Swagger-API文档生成框架的基本使用][Swagger-API 7]

2.2.8 访问swagger-ui

> 访问项目下swagger中index.html,发现也可以达到效果

![Swagger-API文档生成框架的基本使用][Swagger-API 8]

**3.总结**

> 总结下这次初步学习swagger，确实踩��很多坑，有些问题从网上找到的解决方法在自己这里也不管用。总结一下其实如果项目多，单独搭建一个提供查看接口文档的swagger-ui项目，其他项目主要负责生成对应数据格式文件，还是很方便的。有些小伙伴项目嵌入swagger可能会遇到一些小问题，比如生成的文档格式数据、或者原始方式引用swagger-ui被过滤器拦截导致接口生成数据无法读取成功等等。总结���下学习技术还是���静下心来，心越乱离成功越远。下面列一些学习过程中看到的不错的博客推荐给大家，分享给大家一起学习，这应该也是博主写博客的其中一个目的吧。

**相关博客推荐:**

> \[Swagger使用\](https://blog.csdn.net/blucelee2/article/details/51140587)
> 
> \[swagger的使用\](https://blog.csdn.net/gyb\_csdn/article/details/75123575)
> 
> \[swagger编写规范\](https://blog.csdn.net/xxoo00xx00/article/details/77278510)
> 
> \[Swagger框架学习分享\](https://blog.csdn.net/u010827436/article/details/44417637)
> 
> \[swagger2常用注解说明\](https://blog.csdn.net/u014231523/article/details/76522486)
> 
> \[使用springfox+swagger2书写API文档\](https://blog.csdn.net/u012476983/article/details/54090423)
> 
> \[ SpringMVC+Swagger UI生成可视图的API文档(详细图解)\](https://blog.csdn.net/u011499992/article/details/53455144)
> 
> \[Restful形式接口文档生成之Swagger与SpringMVC��合手记\](https://blog.csdn.net/linlzk/article/details/50728264)
> 
> \[SSM三大框架整合Springfox(Swagger2)步骤以及遇到的一些问题\](https://blog.csdn.net/twomr/article/details/77101092)


[Swagger-API]: /pro/os/crawler/BMEN-ZAUN-Y6ZN.jpg
[Swagger-API 1]: /pro/os/crawler/ZFVE-EAYA-FQYN.jpg
[Swagger-API 2]: /pro/os/crawler/BQMI-AQM6-ZFRN.jpg
[Swagger-API 3]: /pro/os/crawler/JJBZ-FBMF-MAQF.jpg
[Swagger-API 4]: /pro/os/crawler/3QZN-ZMZB-MM3Q.jpg
[Swagger-API 5]: /pro/os/crawler/RQMN-ZQVU-UIQZ.jpg
[Swagger-API 6]: /pro/os/crawler/MUMM-Z33Y-2YFA.jpg
[Swagger-API 7]: /pro/os/crawler/EIVJ-R3F6-7J3U.jpg
[Swagger-API 8]: /pro/os/crawler/7RR3-YBEI-UIV2.jpg
 *  **原文作者：** 丶湾里小白龍
 *  **原文链接：** https://www.toutiao.com/item/6558799291660370446/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。