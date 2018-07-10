---
title: 前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）
date: 2017-10-14 06:41:07
categories: "开发"
tags:
	- iPhone
	- 脚本语言
	- BSD
	- HTML5
	- 人工智能

---

> tracking.js是一个开源（BSD协议）的计算机视觉插件，在不同的浏览器中有不同的计算机视觉算法和技术，通过使用现代HTML5规范，能够实现实时颜色跟踪、人脸检测等功能，界面直观、核心文件轻量。

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js]

1、下载及实例  


**https://github.com/eduardolundgren/tracking.js**

首先，下载这个项目，这个项目包括所有的tracking.js的例子、源代码、依赖等。解压把文件放到项目的任意位置， 在页面中引入tracking-min.js文件。

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 1]

然后，在页面中创建img和canvas元素，img是需要识别的图片，canvas识别后生成图片所需容器。

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 2]

最后，运行如下脚本代码即可实现一个简单的图片特征识别。  


![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 3]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 4]

npm安装命令：npm install tracking

bower安装命令：bower install tracking

2、基础功能展示  


①检测视频中的颜色

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 5]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 6]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 7]

②人脸检测  


人脸检测需要额外引入face-min.js文件。

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 8]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 9]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 10]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 11]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 12]

③检测脸、眼睛和嘴  


和上面一样需要额外引入3个文件，分别是face-min.js（脸）、eye-min.js（眼睛）、mouth-min.js（嘴）。

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 13]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 14]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 15]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 16]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 17]

④检测特定的颜色

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 18]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 19]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 20]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 21]

⑤两幅图相似点匹配

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 22]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 23]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 24]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 25]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 26]

⑥使用摄像头检测人脸

摄像头相关的都需要引入dat.gui.min.js文件。

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 27]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 28]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 29]

⑦摄像头图像特征

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 30]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 31]

![前端开发：一个在web上实现计算机视觉的现代方法（tracking.js）][web_tracking.js 32]

这是摄像头拍到苹果手机部分背面的特征点

查看更多例子请前往官方网站：

**https://trackingjs.com/examples/face\_tag\_friends.html**

--------------------

有哪里写得不好的地方请大家提出来，请轻喷，谢谢。 同时有什么与编程相关的好东西可以推举给我，再次感谢！


[web_tracking.js]: /pro/os/crawler/RRQJ-FVZB-RMBE.jpg
[web_tracking.js 1]: /pro/os/crawler/YMRQ-VIII-3EIJ.jpg
[web_tracking.js 2]: /pro/os/crawler/YJYV-ERFF-JMQV.jpg
[web_tracking.js 3]: /pro/os/crawler/ZM3Y-M3F3-2AN2.jpg
[web_tracking.js 4]: /pro/os/crawler/YA6B-QFNN-RNU2.jpg
[web_tracking.js 5]: /pro/os/crawler/RERQ-FUAB-FRZJ.jpg
[web_tracking.js 6]: /pro/os/crawler/ZYIV-AJUR-MMV3.gif
[web_tracking.js 7]: /pro/os/crawler/ARNM-QJ2E-AN7R.jpg
[web_tracking.js 8]: /pro/os/crawler/YRZR-INAR-VNRA.jpg
[web_tracking.js 9]: /pro/os/crawler/N7RI-VJBI-FVEQ.jpg
[web_tracking.js 10]: /pro/os/crawler/RMFA-QRJM-3UUF.jpg
[web_tracking.js 11]: /pro/os/crawler/ZEBV-MYEE-YEF2.jpg
[web_tracking.js 12]: /pro/os/crawler/BYII-YNAY-NUUU.jpg
[web_tracking.js 13]: /pro/os/crawler/MQAQ-ZIU7-J2UU.jpg
[web_tracking.js 14]: /pro/os/crawler/IRMV-EAFU-AEJB.jpg
[web_tracking.js 15]: /pro/os/crawler/BIEF-INFQ-JZMQ.jpg
[web_tracking.js 16]: /pro/os/crawler/EEU6-RMI2-MMJM.jpg
[web_tracking.js 17]: /pro/os/crawler/BR3Q-ZZA2-IV7F.jpg
[web_tracking.js 18]: /pro/os/crawler/YUAY-F3IA-MUIM.jpg
[web_tracking.js 19]: /pro/os/crawler/VY3E-R2JQ-3QQM.jpg
[web_tracking.js 20]: /pro/os/crawler/VYYI-22ZQ-NZA3.jpg
[web_tracking.js 21]: /pro/os/crawler/3I3M-BBZB-FREI.jpg
[web_tracking.js 22]: /pro/os/crawler/YIMZ-JRFN-Q2IZ.jpg
[web_tracking.js 23]: /pro/os/crawler/ZQ3Y-VEFI-VRAA.jpg
[web_tracking.js 24]: /pro/os/crawler/2QNB-QMJB-QMBU.jpg
[web_tracking.js 25]: /pro/os/crawler/MYYZ-737V-RJZZ.jpg
[web_tracking.js 26]: /pro/os/crawler/QBVJ-IY7V-MBQA.jpg
[web_tracking.js 27]: /pro/os/crawler/U7VF-M3RI-UZVY.jpg
[web_tracking.js 28]: /pro/os/crawler/Y6NZ-VBM7-RZYU.jpg
[web_tracking.js 29]: /pro/os/crawler/VJAZ-NIN2-AU7F.jpg
[web_tracking.js 30]: /pro/os/crawler/EYJR-IAAY-267J.jpg
[web_tracking.js 31]: /pro/os/crawler/Y2AR-JYZ2-IFM2.jpg
[web_tracking.js 32]: /pro/os/crawler/FAEV-Q2YA-NAAB.jpg
 *  **原文作者：** IT痕迹
 *  **原文链接：** https://www.toutiao.com/item/6476217962679239181/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。