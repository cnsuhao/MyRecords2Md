---
title: WEB前端工程师整理的原生JavaScript经典百例
date: 2018-04-24 15:39:58
categories: "开发"
tags:
	- CSS
	- JavaScript
	- 编程语言
	- HTML
	- 工程师

---


![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript]

一、原生JavaScript实现字符串长度截取

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 1]

二、原生JavaScript获取域名主机

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 2]

三、原生JavaScript转义html标签

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 3]

四、原生JavaScript时间日期格式替换

Date.prototype.Format = function(formatStr) \{

var str = formatStr;

var Week = \['日', '一', '二', '三', '四', '五', '六'\];

str = str.replace(/yyyy|YYYY/, this.getFullYear());

str = str.replace(/yy|YY/, (this.getYear() % 100) > 9 ? (this.getYear() % 100).toString() : '0' + (this.getYear() % 100)); str = str.replace(/MM/, (this.getMonth() + 1) > 9 ? (this.getMonth() + 1).toString() : '0' + (this.getMonth() + 1));

str = str.replace(/M/g, (this.getMonth() + 1));

str = str.replace(/w|W/g, Week\[this.getDay()\]);

str = str.replace(/dd|DD/, this.getDate() > 9 ? this.getDate().toString() : '0' + this.getDate());

str = str.replace(/d|D/g, this.getDate());

str = str.replace(/hh|HH/, this.getHours() > 9 ? this.getHours().toString() : '0' + this.getHours());

str = str.replace(/h|H/g, this.getHours());

str = str.replace(/mm/, this.getMinutes() > 9 ? this.getMinutes().toString() : '0' + this.getMinutes());

str = str.replace(/m/g, this.getMinutes());

str = str.replace(/ss|SS/, this.getSeconds() > 9 ? this.getSeconds().toString() : '0' + this.getSeconds());

str = str.replace(/s|S/g, this.getSeconds());

return str

五、原生JavaScript判断是否为数字类型

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 4]

六、原生JavaScript获取cookie值

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 5]

七、原生JavaScript格式化CSS样式代码

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 6]

八、原生JavaScript实现checkbox全选与全不选

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 7]

九、原生JavaScript根据样式名称检索元素对象

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 8]

十、原生JavaScript判断是否以某个字符串开头

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 9]

十一、原生JavaScript获取页面scrollLeft

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 10]

十二、原生JavaScript跨浏览器添加事件

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 11]

十三、原生JavaScript用正则表达式清除相同的数组(高效率)

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 12]

篇幅有限，今天先分享这么多，如果大家喜欢的话我会再更新，正在学习WEB前端技术的小伙伴可以进群（669424998）一起交流学习，群里还有大量学习资料可供大家自行下载参看，欢迎大家一起来交流讨论。。

![WEB前端工程师整理的原生JavaScript经典百例][WEB_JavaScript 13]


[WEB_JavaScript]: /pro/os/crawler/7REZ-JMBM-ZVNQ.jpg
[WEB_JavaScript 1]: http://p1.pstatp.com/large/pgc-image/15245549947453ee223b79d
[WEB_JavaScript 2]: http://p9.pstatp.com/large/pgc-image/1524555013284be35978ff8
[WEB_JavaScript 3]: http://p1.pstatp.com/large/pgc-image/152455503053438ee36af64
[WEB_JavaScript 4]: http://p1.pstatp.com/large/pgc-image/15245551759037266ebf786
[WEB_JavaScript 5]: http://p3.pstatp.com/large/pgc-image/15245551935014c6a12c42e
[WEB_JavaScript 6]: http://p1.pstatp.com/large/pgc-image/15245552185809bb8080aec
[WEB_JavaScript 7]: http://p9.pstatp.com/large/pgc-image/1524555240806096ed37ab5
[WEB_JavaScript 8]: http://p3.pstatp.com/large/pgc-image/1524555282162b5f027a637
[WEB_JavaScript 9]: http://p3.pstatp.com/large/pgc-image/15245553023943630c6807d
[WEB_JavaScript 10]: http://p1.pstatp.com/large/pgc-image/1524555345443cb8326dc2f
[WEB_JavaScript 11]: http://p3.pstatp.com/large/pgc-image/1524555367418addccf3a29
[WEB_JavaScript 12]: http://p3.pstatp.com/large/pgc-image/1524555432045e4e9fae9f5
[WEB_JavaScript 13]: http://p3.pstatp.com/large/4e7600035a856391bd14
 *  **原文作者：** 我爱编程
 *  **原文链接：** https://www.toutiao.com/item/6547916438009545223/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。