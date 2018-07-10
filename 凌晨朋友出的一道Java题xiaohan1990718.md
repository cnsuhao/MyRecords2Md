---
title: 凌晨朋友出的一道Java题
date: 2012-11-04 12:17:31
categories: "开发"
tags:
	- Java

---

import java.util.\*;


public class FanXingArray \{
public static void main(String\[\] args)\{
List<String> \[\] array = (List<String> \[\])new ArrayList\[6\];
Object\[\] obj = array;
obj\[0\] = new ArrayList<Integer>();
array\[0\].add("hello");
ArrayList<Integer> a = (ArrayList<Integer>)obj\[0\];
a.add(new Integer(8));
//Integer b = a.get(0);
System.out.println(a.get(0));
\}


\}





这段代码里面，array是String类型的，obj是Object类型的，a是Integer类型的。obj\[0\] = new ArrayList<Integer>();
array\[0\].add("hello");
ArrayList<Integer> a = (ArrayList<Integer>)obj\[0\];
这里为什么没有报错。相当于将字符串hello转换成了Integer类型，为什么没有报错。如果不报错，注释那行为什么会报错呢。还有，为什么a.get(0)为什么没有报错


\---------------------------------------------------------割---------------------------------------------------------------

原因呢？先更新博文，原因再做更新。

我个人认为：声明array是字符类型的数组 然后obj复制array对象内存的引用。 之后给obj\[0\]对象的第一位的地址指向一个整型数组。再给array\[0\]赋值hello 这时 obj\[0\]的内存指向也是指向hello。往下走到a 将ojb\[0\]的地址指向给a 默认是a\[0\]的地址指向hello 它们都是引用类型 未涉及到转换问题 因而不出错。 a.add 向a添加8 输出a.get(0) 默认调用toString方法 故而不报错。 Integer b = a.get(0) 涉及到类型转换 因而报错。


大家可以debug调试下。

![IQ2E-Y3ZY-QFZR.jpg][]


\-------以上解释如有问题 请指正！谢谢。



[IQ2E-Y3ZY-QFZR.jpg]: /pro/os/crawler/IQ2E-Y3ZY-QFZR.jpg