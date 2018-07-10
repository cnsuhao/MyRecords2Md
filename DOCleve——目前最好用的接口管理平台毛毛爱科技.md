---
title: DOCleve——目前最好用的接口管理平台
date: 2017-10-19 11:11:28
categories: "开发"
tags:
	- Word
	- 可视化
	- GitHub
	- JSON
	- HTML

---

DOClever是一款旨在解决接口管理，数据生成，自动化测试的一体化接口平台。在我们平时的开发中，经常会面临前后端接口交互的问题，以往的解决方案多是通过word文档来记录下接口的变更，然后发送给团队成员，这种方式不仅效率地下，而且很容易出错，因此，DOClever就是为了解决这个痛点的一款工具，他不仅集成了接口编辑的分组和管理，在接口测试上也是独居特色，采用的是后端proxy代理，无需任何插件，接口数据实时展示！  


![DOCleve——目前最好用的接口管理平台][DOCleve]

现状

DOClever自今年年初上线以来，一直秉承着开源免费的宗旨，至今已积累用户8000+，项目10000+，这些都是线上平台的数据，我们提供的全功能线下部署版本还不包含在内。目前已和滴滴，同程等互联网公司深度合作，得到了他们的支持，在此很感谢大家对我们的信任和肯定，目前平台服务很稳定，大家可以放心使用！

链接和文档

官网： http://doclever.cn/

Github: https://github.com/sx1989827/DOClever

码云: https://gitee.com/sx1989827/SBDoc

视频文档: http://doclever.cn/help/help.html

关于DOClever

特性

 *  可以对接口信息进行编辑管理，支持get,post,put,delete,patch五种方法，支持https和https协议，并且支持query，body，json，raw，rest，formdata的参数可视化编辑。同时对json可以进行无限层次可视化编辑。并且，状态码，代码注入，markdown文档等附加功能应有尽有。
 *  接口调试运行，一个都不能少，可以对参数进行加密，从md5到aes一应俱全，返回参数与模型实时分析对比，给出不一致的地方，找出接口可能出现的问题。如果你不想手写文档，那么试试接口的数据生成功能，可以对接口运行的数据一键生成文档信息。
 *  mock的无缝整合，DOClever自己就是一个mock服务器，当你把接口的开发状态设置成已完成，本地mock便会自动请求真实接口数据，否则返回事先定义好的mock数据。
 *  支持postman，rap，swagger的导入，方便你做无缝迁移，同时也支持html文件的导出，方便你离线浏览！
 *  项目版本和接口快照功能并行，你可以为一个项目定义1.0，1.1，1.2版本，并且可以自由的在不同版本间切换回滚，再也不怕接口信息的遗失，同时接口也有快照功能，当你接口开发到一半或者接口需求变更的时候，可以随时查看之前编辑的接口信息。
 *  自动化测试功能，目前市面上类似平台的接口自动化测试大部分都是伪自动化，对于一个复杂的场景，比如获取验证码，登陆，获取订单列表，获取某个特定订单详情这样一个上下文关联的一系列操作无能为力。而DOClever独创的自动化测试功能，只需要你编写极少量的javascript代码便可以在网页里完成这样一系列操作，同时，DOClever还提供了后台定时批量执行测试用例并把结果发送到团队成员邮箱的功能，你可以及时获取接口的运行状态。
 *  团队协作功能，很多类似的平台这样的功能是收费的，但是DOClever觉得好东西需要共享出来，你可以新建一个团队，并且把团队内的成员都拉进来，给他们分组，给他们分配相关的项目以及权限，发布团队公告等等。
 *  DOClever开源免费，支持内网部署，很多公司考虑到数据的安全性，不愿意把接口放到公网上，没有关系，DOClever给出一个方便快捷的解决方案，你可以把平台放到自己的内网上，完全不需要连接外网，同时功能一样也不少，即便是对于产品的升级，DOClever也提供了很便捷的升级方案！

首页

![DOCleve——目前最好用的接口管理平台][DOCleve 1]

项目列表

![DOCleve——目前最好用的接口管理平台][DOCleve 2]

项目首页

![DOCleve——目前最好用的接口管理平台][DOCleve 3]

接口测试

![DOCleve——目前最好用的接口管理平台][DOCleve 4]

自动化测试

![DOCleve——目前最好用的接口管理平台][DOCleve 5]

团队管理

![DOCleve——目前最好用的接口管理平台][DOCleve 6]

部分使用企业

![DOCleve——目前最好用的接口管理平台][DOCleve 7]

内网部署

 *  内网部署版本免费开源，功能和线上版本完全一致
 *  内网部署版本使用mongodb作为数据库，由node统一启动，node版本为最新的lts版本
 *  具体部署步骤请点击这里
 *  DOClever单独提供了docker的部署版本，链接


[DOCleve]: /pro/os/crawler/MQYA-J3R7-ZUV2.jpg
[DOCleve 1]: /pro/os/crawler/IJJV-AY3Q-RYME.jpg
[DOCleve 2]: /pro/os/crawler/Y3QM-JFVV-I322.jpg
[DOCleve 3]: /pro/os/crawler/FIIV-Y2EZ-JZZM.jpg
[DOCleve 4]: /pro/os/crawler/EJVM-FIA6-F2AF.jpg
[DOCleve 5]: /pro/os/crawler/BVFR-MFRF-ZJJ3.jpg
[DOCleve 6]: /pro/os/crawler/2MEF-RF6V-IEYF.jpg
[DOCleve 7]: /pro/os/crawler/JAQB-JRNU-3YYV.jpg
 *  **原文作者：** 毛毛爱科技
 *  **原文链接：** https://www.toutiao.com/item/6478453278781735437/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。