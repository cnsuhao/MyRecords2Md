---
title: 一个网页的对象抽象之路——po编程（干货，Java自动化测试）
date: 2018-05-23 16:00:33
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言
	- 面向对象程序编程

---

先来看一个在腾讯课堂首页搜索机构的操作步骤：

1：首先打开腾讯课堂的首页：https://ke.qq.com

2：点击课程或机构的下拉选择图标

3：选择机构

4：在搜索框输入要搜索的机构名称

5：点击查找图标查找机构，跳转到查找结果页面

6：检查查找出的机构名称

7：点击机构logo跳转详情页面

上述操作涉及到两个页面，腾讯课堂首页和搜索结果页，操作图示如下：

1：腾讯课堂首页

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java]

2：搜索结果页面

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 1]

使用webdirver的api完成模拟上述步骤代码如下：

SearchCase.java

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 2]

上述代码简单而清晰，但如果每个测试用例都这样去写的话，将会出现非常多的重复性代码，可维护性较低，因此，在使用Selenium WebDirver进行自动化开发时，可以使用po（页面对象）编程的思想，减少重复性代码、提高代码的可维护性。

一：po的概念与思想

**po是page object的缩写，即页面对象。使用po是对页面进行抽象或者说建模的过程，需要把页面当作一个对象**。

面向对象编程语言中，进行面向对象编程需要考虑以下两点：

1：对象的属性（全局变量）

2：对象的行为（函数）

po思想也是一样，对页面进行抽象时，把页面的一个一个的web元素设计为页面对象的属性，把页面上的操作（如点击、输入等）设计为页面对象的行为

二：po中元素的定位

Selenium提供了许多注解和Api可以方便的定位元素和初始化元素，如下是腾讯课堂首页下拉选择机构图标的声明方式：

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 3]

三行代码释义：

1、@FindBy(css="i\[class='icon-font i-v-bottom-small'\]")

指定了要元素的定位方式，如上表示以cssSelector方式进行定位，还有其他7种写法@FindBy(id="xxx")、@FindBy(name="xxx")...

2、@CacheLookup

表示缓存查找到元素，重复使用可提高查询速度

**3、public** WebElement select\_icon;

声明一个web元素类型的全局变量

三：po中元素的初始化

Po提供了 PageFactory.initElements()来初始化页面元素，把查找的元素赋值到我们定义的属性（全局变量）上面

为了非常方便的进行页面元素的初始化，我们把该方法放置到页面类型的构造方法中，当调用该构造方法创建页面对象时，会调用该方法同时初始化页面的元素

代码如下：

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 4]

四：页面行为抽象

元素初始化后，通过操作元素（如点击，输入）即可完成页面的功能，po中对页面功能的抽象，则提现为声明一个一个的对象方法

如下是页面选择机构的行为的抽象：

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 5]

下面是对“腾讯课堂首页”页面进行抽象的完整代码：

KeqqHomePage.java

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 6]

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 7]

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 8]

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 9]

其中继承的父类BasePage仅仅只包含一个静态全局变量driver，方便其他页面继承直接使用

BasePage.java

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 10]

其中BrowerSelector.getWebDriver()方法是封装好的一个工具类，用来启动浏览器，得到driver对象用的，在后面的源代码网盘链接中可以下载到。

相同的方式对“结果页面”进行抽象,代码如下：

SearchResultPage.java

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 11]

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 12]

执行测试的类，代码如下：

Tester.java

![一个网页的对象抽象之路——po编程（干货，Java自动化测试）][po_Java 13]

总结：

po编程，实质上就是分离页面元素、封装页面元素操作的过程，将可能变化的页面元素单独映射到对象属性、将页面元素操作抽象成函数，让我们以面向对象方式理解抽象一个页面，实际上就是编程中解耦思想的应用，从而简化测试用例的步骤，提高自动化测试代码的可维护性。


[po_Java]: /pro/os/crawler/ZZQY-NRR7-ZFZY.jpg
[po_Java 1]: /pro/os/crawler/VABB-MQRI-AE32.jpg
[po_Java 2]: /pro/os/crawler/FRIU-IYJJ-JVVB.jpg
[po_Java 3]: /pro/os/crawler/BARZ-FY6F-UMMU.jpg
[po_Java 4]: /pro/os/crawler/NABJ-VYMN-BINY.jpg
[po_Java 5]: /pro/os/crawler/3AMQ-QMMZ-FBQR.jpg
[po_Java 6]: /pro/os/crawler/2INZ-Z2QQ-RMVU.jpg
[po_Java 7]: /pro/os/crawler/3YFR-VRFR-MBFB.jpg
[po_Java 8]: /pro/os/crawler/MJYI-BZUI-7ZFJ.jpg
[po_Java 9]: /pro/os/crawler/MMA3-E3V2-AJN3.jpg
[po_Java 10]: /pro/os/crawler/AZ7R-U2MN-RJIQ.jpg
[po_Java 11]: /pro/os/crawler/ZVJJ-EEF3-EUM2.jpg
[po_Java 12]: /pro/os/crawler/JVN7-NNMR-NZZF.jpg
[po_Java 13]: /pro/os/crawler/YJMM-RAZ6-JFAM.jpg
 *  **原文作者：** 柠檬班软件测试
 *  **原文链接：** https://www.toutiao.com/item/6558683209431794179/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。