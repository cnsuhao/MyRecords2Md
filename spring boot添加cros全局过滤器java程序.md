---
title: spring boot添加cros全局过滤器
date: 2018-01-29 21:43:06
categories: "开发"
tags:
	- 技术
	- HTML

---

 *  新增一个过滤器类并实现filter接口


![spring boot添加cros全局过滤器][spring boot_cros]

@Bean

@Order(1)

public FilterRegistrationBean crosFilter() \{

System.out.println("初始化跨域过滤器开始==================================");

final FilterRegistrationBean registrationBean = new FilterRegistrationBean();

com.liushun.common.filter.CorsFilter crCorsFilter = new com.liushun.common.filter.CorsFilter();

registrationBean.setFilter(crCorsFilter);

return registrationBean;

\}

架构师视频资料分享链接：

data:text/html;charset=UTF-8;base64,

5p625p6E5biI5a2m5Lmg5Lqk5rWB576k5Y+35pivNjk2NTU1NTMyCg==

复制粘贴在网站即可！


[spring boot_cros]: /pro/os/crawler/QABU-NNBA-ZINY.jpg
 *  **原文作者：** java程序
 *  **原文链接：** https://www.toutiao.com/item/6516467772057190920/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。