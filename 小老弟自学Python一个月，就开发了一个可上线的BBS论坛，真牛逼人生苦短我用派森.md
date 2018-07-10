---
title: 小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼
date: 2018-06-29 23:31:01
categories: "开发"
tags:
	- jQuery
	- Django
	- 编程语言
	- HTML
	- Python

---

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS]

**需求**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 1]

**知识点**

Django、HTMLCSSJS、BootStrap、Jquery。

**设计表结构**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 2]

# **CSRF** #

**CSRF 简介**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 3]

**CSRF 攻击实例**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 4]

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 5]

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 6]

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 7]

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 8]

**CSRF 攻击的对象**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 9]

**防御策略：在请求地址中添加 token 并验证**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 10]

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 11]

**Django 中使用CSRF**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 12]

# **上传文件** #

**template form表单** 

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 13]

# **多级评论** #

**用户可以直接在帖子内评论，别人也可以评论我们的评论，就是盖楼，如图：**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 14]

**所有的评论都在一张表中了， 评论与评论之前又有从属关系，那么怎么样才能在前端 页面上把这种层级关系体现出来？**

**我们先把评论简化成一个这样的模型：**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 15]

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 16]

**自定义template tag模块**

![小老弟自学Python一个月，就开发了一个可上线的BBS论坛，真牛逼][Python_BBS 17]

很多小伙伴都经常抱怨说没有找到合适的学习资料，大家可以在后台私信回复我01，系统自动发送书籍和视频资料哦


[Python_BBS]: /pro/os/crawler/AYUF-RMJZ-MRER.jpg
[Python_BBS 1]: /pro/os/crawler/AI7R-YEU6-BQFQ.jpg
[Python_BBS 2]: /pro/os/crawler/ENZI-32RQ-BBF3.jpg
[Python_BBS 3]: /pro/os/crawler/VAFF-IEF7-7RA2.jpg
[Python_BBS 4]: /pro/os/crawler/JUM2-UZ7N-YREE.jpg
[Python_BBS 5]: /pro/os/crawler/Q6RU-VMMB-AIBF.jpg
[Python_BBS 6]: /pro/os/crawler/RVUA-6BZU-FRRB.jpg
[Python_BBS 7]: /pro/os/crawler/INRJ-FIFN-BR3Y.jpg
[Python_BBS 8]: /pro/os/crawler/AJFA-IBUE-AYZF.jpg
[Python_BBS 9]: /pro/os/crawler/UZJU-6ZQM-IEUB.jpg
[Python_BBS 10]: /pro/os/crawler/ABU7-RFEJ-BZUU.jpg
[Python_BBS 11]: /pro/os/crawler/BENF-FRMF-FYNJ.jpg
[Python_BBS 12]: /pro/os/crawler/VMYE-ZF7R-ANI2.jpg
[Python_BBS 13]: /pro/os/crawler/RFZE-RQZY-AEYF.jpg
[Python_BBS 14]: /pro/os/crawler/FU2A-JARF-ZRZ3.jpg
[Python_BBS 15]: /pro/os/crawler/ZMF3-IVAF-JEEV.jpg
[Python_BBS 16]: /pro/os/crawler/VUQU-MMJU-AFUF.jpg
[Python_BBS 17]: /pro/os/crawler/MZV6-RMIF-YQEI.jpg
 *  **原文作者：** 人生苦短我用派森
 *  **原文链接：** https://www.toutiao.com/item/6572529446106956291/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。