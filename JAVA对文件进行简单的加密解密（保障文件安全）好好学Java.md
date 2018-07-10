---
title: JAVA对文件进行简单的加密解密（保障文件安全）
date: 2018-05-13 06:00:00
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言
	- 托福

---

# 1、文件的重要性  #

在我们构建任何系统的时候，都要考虑到系统的安全性。针对文件保密性比较高的系统，我们要采取加密解密操作，其实就是编解码的过程。


![JAVA对文件进行简单的加密解密（保障文件安全）][JAVA]

文件加密

# 2、设计思路 #

通过提取文件流，对流中读取到的数据进行处理，然后写入到新的文件当中去。当解密的时候采取对应的方式进行数据处理。


# 3、准备test.txt文件  #

![JAVA对文件进行简单的加密解密（保障文件安全）][JAVA 1]

# 4、加密实现  #

![JAVA对文件进行简单的加密解密（保障文件安全）][JAVA 2]

加密实现

![JAVA对文件进行简单的加密解密（保障文件安全）][JAVA 3]

获得乱码的加密数据

# 5、解密实现 #

![JAVA对文件进行简单的加密解密（保障文件安全）][JAVA 4]

解密实现

![JAVA对文件进行简单的加密解密（保障文件安全）][JAVA 5]

解密文件与原文件一致

# 6、总结  #

简单的加密解密实现，仅供参考。在加密解密中用到ibt加一个数和减一个固定的数，这里是7。如实际操作中，可每个文件在数据库里绑定一个随机数。以防止算法被破解。今天是母情节，愿全天下母亲健健康康，开心快乐，快给妈妈送祝福吧！



[JAVA]: /pro/os/crawler/QZRE-VYJM-RVVZ.jpg
[JAVA 1]: /pro/os/crawler/IAF2-IERV-IFN2.jpg
[JAVA 2]: /pro/os/crawler/UIZA-2QFV-J2EB.jpg
[JAVA 3]: /pro/os/crawler/Q3IQ-MQQN-3QBQ.jpg
[JAVA 4]: /pro/os/crawler/AFNU-NZRR-ZNJE.jpg
[JAVA 5]: /pro/os/crawler/VUVY-AV3U-MZJF.jpg
 *  **原文作者：** 好好学Java
 *  **原文链接：** https://www.toutiao.com/item/6554708170059547149/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。