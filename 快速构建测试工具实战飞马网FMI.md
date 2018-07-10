---
title: 快速构建测试工具实战
date: 2018-02-08 11:45:45
categories: "开发"
tags:
	- 脚本语言
	- 需求分析
	- Velocity
	- 工程师
	- 云计算

---

2018年1月18日，来自京东的美女测试开发工程师胡文萍为大家在线讲解了如何快速构建测试工具的一些实战。

![快速构建测试工具实战][2QAE-63B3-6BME.jpg]

胡文萍

胡文萍，京东商城-POP平台-测试与质量管理部高级测试开发工程师。从事电商平台自动化框架及开发工具工作多年，对工具开发过程及运维等实战经验丰富。

**快速构建质量保障工具概览**

一、成熟的技术栈

后端框架：Spring、SpringMVC、SpringBoot、Mybatis；

前端框架：Velocity、JQuery、Vue、BootStrap；

二、灵活强大的中间件

Redis、Kafka、云存储等；

三、严谨公开的开发流程管理

需求调研、开发设计；

四、全栈的工作模式；

**需求分析----报告平台**

正向流程梳理：走访需求方，梳理用户需求表；

页面原型输出：理解用户需求，输出用户原型；

可实现新需求讨论：关键技术选型及可实现新讨论；

**原型输出：**

**参与人员：**前端开发、需求提出方；

**讨论内容：**确定需要的页面，如何交互、用户入户等；

**输出：**系统原型（静态页或者Axure原型）；

![快速构建测试工具实战][BFYJ-ZUIR-VUFJ.jpg]

![快速构建测试工具实战][IZMQ-QBER-Q7N2.jpg]

**需求分析****----技术选型及阻塞点排查**

1、数据量大可能导致的整体查询性能问题。如历史数据单独存储；Join、where判断字段增加索引；

2、异常栈数据如何存储。传送到云存储系统、数据库记录云端链接；

3、Case信息如何扫描。脚本规范，通过@descriptor获取case描述；worker定时扫描；

4、RPC框架下，如何实时获取执行数据。自动化平台发布写入接口，自动化脚本框架中加入listener监听。

![快速构建测试工具实战][IRE7-Z3UV-ZFR3.jpg]

![快速构建测试工具实战][7BBR-YU3E-YRFM.jpg]

![快速构建测试工具实战][VMVJ-VYZB-FFFQ.jpg]

![快速构建测试工具实战][YNZE-M2ZB-3YNR.jpg]

![快速构建测试工具实战][MM3Q-RRQN-AMYV.jpg]

![快速构建测试工具实战][AYIY-MAQU-EQIQ.jpg]

![快速构建测试工具实战][2AZE-UVJU-6VUU.jpg]

![快速构建测试工具实战][RRMJ-RAM7-RIAM.jpg]

![快速构建测试工具实战][B6JB-EMFZ-BFZU.jpg]


[2QAE-63B3-6BME.jpg]: /pro/os/crawler/2QAE-63B3-6BME.jpg
[BFYJ-ZUIR-VUFJ.jpg]: /pro/os/crawler/BFYJ-ZUIR-VUFJ.jpg
[IZMQ-QBER-Q7N2.jpg]: /pro/os/crawler/IZMQ-QBER-Q7N2.jpg
[IRE7-Z3UV-ZFR3.jpg]: /pro/os/crawler/IRE7-Z3UV-ZFR3.jpg
[7BBR-YU3E-YRFM.jpg]: /pro/os/crawler/7BBR-YU3E-YRFM.jpg
[VMVJ-VYZB-FFFQ.jpg]: /pro/os/crawler/VMVJ-VYZB-FFFQ.jpg
[YNZE-M2ZB-3YNR.jpg]: /pro/os/crawler/YNZE-M2ZB-3YNR.jpg
[MM3Q-RRQN-AMYV.jpg]: /pro/os/crawler/MM3Q-RRQN-AMYV.jpg
[AYIY-MAQU-EQIQ.jpg]: /pro/os/crawler/AYIY-MAQU-EQIQ.jpg
[2AZE-UVJU-6VUU.jpg]: /pro/os/crawler/2AZE-UVJU-6VUU.jpg
[RRMJ-RAM7-RIAM.jpg]: /pro/os/crawler/RRMJ-RAM7-RIAM.jpg
[B6JB-EMFZ-BFZU.jpg]: /pro/os/crawler/B6JB-EMFZ-BFZU.jpg
 *  **原文作者：** 飞马网FMI
 *  **原文链接：** https://www.toutiao.com/item/6520024690897977864/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。