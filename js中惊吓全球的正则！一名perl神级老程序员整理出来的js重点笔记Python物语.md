---
title: js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记
date: 2017-10-26 12:33:07
categories: "开发"
tags:
	- 程序员
	- 编程语言
	- Perl
	- 英语
	- Python

---

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js]

加班赶项目成功跑起来、无BUG的表情

如果遇到很多问题找不到人解决？这里推荐一个群，群内貌似也有两千多人了，有很多热爱python聚集在了一起，并整理了大量的学习资料上传到了群文件当中，喜欢python的朋友可以加入python群：526929231欢迎大家交流讨论各种技术，一起快速成长

# 正则表达式 #

文本的检索与替换功能

# 正则定义 #

 *  /.../gi;
 *  new RegExp('...');

# 正则方法 #

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 1]

# 元字符 #

<table>
 <thead>
  <tr>
   <th>元字符</th>
   <th>描述</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td></td>
   <td>假如要匹配一个反斜杠就需要再添加一个反斜杠标识转义</td>
  </tr>
  <tr>
   <td> </td>
   <td>匹配换行</td>
  </tr>
  <tr>
   <td> </td>
   <td>匹配回车</td>
  </tr>
  <tr>
   <td> </td>
   <td>匹配制表符</td>
  </tr>
  <tr>
   <td>.</td>
   <td>匹配一个任意字符</td>
  </tr>
  <tr>
   <td>*</td>
   <td>匹配零次或多次</td>
  </tr>
  <tr>
   <td>+</td>
   <td>匹配至少一次或多次</td>
  </tr>
  <tr>
   <td>?</td>
   <td>匹配零次或一次</td>
  </tr>
  <tr>
   <td>^</td>
   <td>匹配以什么开头,在子集表示非</td>
  </tr>
  <tr>
   <td>$</td>
   <td>匹配以什么结尾</td>
  </tr>
  <tr>
   <td>d</td>
   <td>匹配数字 [0-9]</td>
  </tr>
  <tr>
   <td>D</td>
   <td>匹配匹配非数字[^d] [^0-9]</td>
  </tr>
  <tr>
   <td>w</td>
   <td>匹配数字字母下划线[A-Za-z0-9_]</td>
  </tr>
  <tr>
   <td>W</td>
   <td>匹配非数字字母下划线[^A-Za-z0-9_]</td>
  </tr>
  <tr>
   <td>s</td>
   <td>匹配任意空格包括Tab和换行</td>
  </tr>
  <tr>
   <td>S</td>
   <td>匹配非空白</td>
  </tr>
  <tr>
   <td></td>
   <td>匹配"英文"单词是否独立部分，除中文</td>
  </tr>
  <tr>
   <td>B</td>
   <td>匹配"英文"单词非独立部分，除中文</td>
  </tr>
 </tbody>
</table>

> 在windows的编辑器中敲一次回车表示 的一个组合

# 标识符/标志符 #

<table>
 <thead>
  <tr>
   <th>标识符</th>
   <th>描述</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>g</td>
   <td>global全局匹配</td>
  </tr>
  <tr>
   <td>i</td>
   <td>ignore忽略大小写</td>
  </tr>
  <tr>
   <td>m</td>
   <td>多行匹配，不会用到此模式，不如全局模式匹配</td>
  </tr>
  <tr>
   <td>y</td>
   <td>ES6新加入的正则匹配模式：粘滞</td>
  </tr>
 </tbody>
</table>

# 量词 #

<table>
 <thead>
  <tr>
   <th>量词</th>
   <th>描述</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>{5}</td>
   <td>匹配五次前面的元素</td>
  </tr>
  <tr>
   <td>{5,}</td>
   <td>匹配前面的元素至少匹配五次，最多不限制</td>
  </tr>
  <tr>
   <td>{0,5}</td>
   <td>匹配前面的元素至少0次，最多5次</td>
  </tr>
  <tr>
   <td>{3,5}</td>
   <td>匹配前面的元素至少匹配3次，最多5次</td>
  </tr>
 </tbody>
</table>

# 字符集 #

<table>
 <thead>
  <tr>
   <th>字符集</th>
   <th>描述</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>[a-z]</td>
   <td>匹配a-z小写的26个英文字母</td>
  </tr>
  <tr>
   <td>[A-Z]</td>
   <td>匹配A-Z大写的26个英文字母</td>
  </tr>
  <tr>
   <td>[0-9]</td>
   <td>匹配A-Z大写的26个英文字母</td>
  </tr>
  <tr>
   <td>[/u4e00-/u9fa5]</td>
   <td>匹配简体中文、繁体字</td>
  </tr>
  <tr>
   <td>[324]</td>
   <td>匹配子集里面任意单个元素</td>
  </tr>
  <tr>
   <td>3|2|4</td>
   <td>匹配子集里面元素，或者匹配3或者匹配2或者匹配4</td>
  </tr>
 </tbody>
</table>

# 子集 #

> var str = 'adb cbd';

(abc)+ 可以控制子集中的匹配次数，abc+ 表示c的一次或多次

<table>
 <thead>
  <tr>
   <th>子集</th>
   <th>描述</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>/(abc)+/g</td>
   <td>表示abc的一坨匹配一次或多次</td>
  </tr>
  <tr>
   <td>/(ab|qb)d/g</td>
   <td>表示或者匹配abd或者匹配qbd</td>
  </tr>
  <tr>
   <td>/(ab)(d)/</td>
   <td>match没有全局匹配的时候，还会返回子集匹配到的字符</td>
  </tr>
 </tbody>
</table>

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 2]

# 参考栗子    #

**邮箱方式**

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 3]

**身份证**

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 4]

**手机号**

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 5]

**匹配密码**

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 6]

# replace替换 #

 *  参一：\[ string、Reg \] 欲查找替换的文本
 *  参二：\[ string、func \] 用作替换的文本或填写函数，通过函数中的return来改变要替换的文本的内容，函数中的第一个形参表示参数一匹配的到的内容，参数二之后表示参数一中的子集、以此类推第二个子集、三个子集

# 输入框的敏感字符过滤 #

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 7]

# call、apply、bind #

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 8]

![js中惊吓全球的正则！一名perl神级老程序员整理出来的js重点笔记][js_perl_js 9]

当然通过bind特性返回新函数，可以扩展一个事件函数传this指向，并且不会执行该函数，再通过事件异步驱动模型的事件方式去执行


[js_perl_js]: /pro/os/crawler/QA2Q-3IIB-FMAB.gif
[js_perl_js 1]: /pro/os/crawler/JZJZ-J3MN-MRJI.jpg
[js_perl_js 2]: /pro/os/crawler/EJJR-YFMM-3EQM.jpg
[js_perl_js 3]: /pro/os/crawler/RNQN-UA6B-3AAU.jpg
[js_perl_js 4]: /pro/os/crawler/ANUM-U2IF-NFRR.jpg
[js_perl_js 5]: /pro/os/crawler/YRVF-B2N7-3YEJ.jpg
[js_perl_js 6]: /pro/os/crawler/VNY2-UU3Q-E2MQ.jpg
[js_perl_js 7]: /pro/os/crawler/AQIR-BIV2-UMIY.jpg
[js_perl_js 8]: /pro/os/crawler/R36V-7VMJ-E2Q3.jpg
[js_perl_js 9]: /pro/os/crawler/ZB7J-RBFZ-A63I.jpg
 *  **原文作者：** Python物语
 *  **原文链接：** https://www.toutiao.com/item/6481072952107336206/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。