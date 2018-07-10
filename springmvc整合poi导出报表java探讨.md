---
title: springmvc整合poi导出报表
date: 2017-04-01 00:17:50
categories: "开发"
tags:
	- Excel

---

前面介绍了使用poi对excel的读和写。接下来介绍一下项目中对其进行导出。

先看目录结构（注：因只是模拟，也为了简单获取数据部分就不操作数据库了）

![springmvc整合poi导出报表][springmvc_poi]

1、Student.java

简单三个属性，两个构造方法，主要是创建时方便用。![springmvc整合poi导出报表][springmvc_poi 1]

2、StudentService.java

![springmvc整合poi导出报表][springmvc_poi 2]

3、StudentServiceImpl.java

![springmvc整合poi导出报表][springmvc_poi 3]

4、StudentController.java

![springmvc整合poi导出报表][springmvc_poi 4]

![springmvc整合poi导出报表][springmvc_poi 5]

5、student.xlsx

![springmvc整合poi导出报表][springmvc_poi 6]

6、执行效果

![springmvc整合poi导出报表][springmvc_poi 7]


[springmvc_poi]: /pro/os/crawler/UQBM-Y2AI-BM3U.jpg
[springmvc_poi 1]: /pro/os/crawler/ZYAV-VZJ2-QMEB.jpg
[springmvc_poi 2]: /pro/os/crawler/QZ3M-6FZ6-7FQY.jpg
[springmvc_poi 3]: /pro/os/crawler/RIIY-NMJQ-UY22.jpg
[springmvc_poi 4]: /pro/os/crawler/ARAA-IVYY-EYMA.jpg
[springmvc_poi 5]: /pro/os/crawler/MIZA-RYIJ-M2EI.jpg
[springmvc_poi 6]: /pro/os/crawler/EMEE-BN2E-AQZZ.jpg
[springmvc_poi 7]: /pro/os/crawler/FEYI-EQFU-UN3A.jpg
 *  **原文作者：** java探讨
 *  **原文链接：** https://www.toutiao.com/item/6403303960681120257/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。