---
title: spring+ Spring cloud + SSO单点登录应用认证
date: 2017-11-07 08:54:33
categories: "开发"
tags:
	- 技术
	- 软件

---

之前的文章中有介绍spring cloud sso集成的方案，也做过spring + jwt + redis的解决方案，不同系统的无缝隙集成，统一的sso单点登录界面的管理、每个应用集成的权限认证，白名单等都是我们需要考虑的，现在针对于以上的问题我们做了sso单点登录应用认证平台，设计如下：

> DROP TABLE IF EXISTS \`sso\_app\_apply\`;
> 
> CREATE TABLE \`sso\_app\_apply\` (
> 
> \`id\` varchar(200) NOT NULL COMMENT '编号',
> 
> \`type\` varchar(200) NOT NULL COMMENT '所属分类',
> 
> \`applicant\` varchar(200) NOT NULL COMMENT '申请人',
> 
> \`approver\` varchar(200) NOT NULL COMMENT '审批人',
> 
> \`appname\` varchar(200) NOT NULL COMMENT '应用名称',
> 
> \`range\` varchar(200) NOT NULL COMMENT '使用范围',
> 
> \`token\` varchar(200) NOT NULL COMMENT 'token认证码',
> 
> \`approval\_time\` datetime NOT NULL COMMENT '审批时间',
> 
> \`create\_date\` datetime NOT NULL COMMENT '创建时间',
> 
> \`update\_by\` varchar(64) NOT NULL COMMENT '更新者',
> 
> \`update\_date\` datetime NOT NULL COMMENT '更新时间',
> 
> \`del\_flag\` char(1) NOT NULL DEFAULT '0' COMMENT '删除标记',
> 
> \`status\` char(1) DEFAULT '0' COMMENT '审核状态：0(待审核) 1(审核通过) 2(驳回) 3(黑名单)',
> 
> PRIMARY KEY (\`id\`)
> 
> ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='sso应用申请表';
> 
> DROP TABLE IF EXISTS \`sso\_app\_template\`;
> 
> CREATE TABLE \`sso\_app\_template\` (
> 
> \`id\` varchar(200) NOT NULL COMMENT '编号',
> 
> \`a\_id\` varchar(200) NOT NULL COMMENT '应用id',
> 
> \`t\_id\` varchar(200) NOT NULL COMMENT '模板id',
> 
> PRIMARY KEY (\`id\`)
> 
> ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='sso应用模板中间表';
> 
> DROP TABLE IF EXISTS \`sso\_template\`;
> 
> CREATE TABLE \`sso\_template\` (
> 
> \`id\` varchar(200) NOT NULL COMMENT '编号',
> 
> \`name\` varchar(200) NOT NULL COMMENT '模板名称',
> 
> \`type\` varchar(200) NOT NULL COMMENT '模板分类',
> 
> \`img\` varchar(200) NOT NULL COMMENT '模板图片',
> 
> \`create\_by\` varchar(64) NOT NULL COMMENT '创建者',
> 
> \`create\_date\` datetime NOT NULL COMMENT '创建时间',
> 
> \`update\_by\` varchar(64) NOT NULL COMMENT '更新者',
> 
> \`update\_date\` datetime NOT NULL COMMENT '更新时间',
> 
> PRIMARY KEY (\`id\`)
> 
> ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='sso模板表';

2. 执行流程

A. 成用户注册 （可以注册个人账户或者企业账户）

B. 申请应用（可能是多个应用），选择不同的模板（不同模板对应不同行业的sso单点登录系统）

C. 管理人员进行应用审核（申请人提交信息的审核），审核通过以后通过加密方式生成应用对应的token信息

D. 后台管理（应用列表、应用审核、模板管理等）

E. 将token信息和应用信息传递，进行sso统一拦截器认证（验证白名单）

F. 成功or失败（跳转到指定模板的sso登录界面）

![spring+ Spring cloud + SSO单点登录应用认证][spring_ Spring cloud _ SSO]

![spring+ Spring cloud + SSO单点登录应用认证][spring_ Spring cloud _ SSO 1]

到此完毕！


[spring_ Spring cloud _ SSO]: /pro/os/crawler/NZMN-MEAJ-ZIFV.jpg
[spring_ Spring cloud _ SSO 1]: /pro/os/crawler/VZFB-IUEI-UREU.jpg
 *  **原文作者：** 猿之家
 *  **原文链接：** https://www.toutiao.com/item/6485469649164042766/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。