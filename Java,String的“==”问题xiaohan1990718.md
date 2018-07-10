---
title: Java,String的“==”问题
date: 2012-11-25 06:59:51
categories: "开发"
tags:
	- Java

---

//时间精力等问题，是前几日写的，部分还没有注释以及来得及查明原因。今日早起更新，出门办事去~

``````````
package mapPractice;

/**
 * 
 * @Detail : String的==一些问题 
 * @Author: 韩庆（QQ:haw_king@foxmail.com）
 * @E-mail: IsaidIwillgoon@gmail.com
 * @Date: 2012-11-20
 * @Time: 下午10:02:46
 *
 */
public class StringTest {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		String str1 = new String("hank");//创建两个不同的对象
		String str2 = new String("hank");
		println("str1==str2  is " + (str1 == str2));//str1与str2指向的地址不同的
		/*
		 * 详解：1、地址的问题  2、执行分配地址的问题  3、无引用地址时字符串相加的问题  ？
		 */
		String str3 = "hank";//字面量值的方式 创建新的“hank”对象。str3指向这个对象的地址
		String str4 = "hank";//java池内检测是否存在，如果存在，那么str4则又指向它
		println("str3==str4 is " + (str3 == str4));
		System.out.println("---"+(str1==str3));
		/**/
		String _str4 = "ha";//字面量方式   创建新的
		String str4_ = "nk";//字面量方式   创建新的
		println("str1 = _str4+str4_ is " + (str1 == _str4 + str4_));
		println("str3 = _str4+str4_ is " + (str3 == _str4 + str4_));
		println("str3 = \"ha\".concat(\"nk\") is "
				+ (str3 == "ha".concat("hk")));
		/**/
		String str5 = _str4 + str4_;
		println("str3 = str5 is " + (str3 == str5));
		/**/
		println("-------------分隔---------------");

		String str4__ = "hank";
		String __str4 = "";
		println("str3 = str4__+__str4 is " + (str3 == str4__ + __str4));
		/**/
		println("切记：如果将上述的()括号去掉，则均返回false，原因是+将前后连接后才跟==号后比较的.");

		println(str3 == "ha" + "nk");
		/**/
		println(str1 == "ha" + "nk");
		/**/
		print(str3 == "hank" + "");
		/**/
		println("-----------分隔-----------");
		StringBuilder sb = new StringBuilder("ha");
		sb.append("nk");
		String str6 = "hank";
		println(str6 == sb.toString());
		println("str3 = \"ha\".concat(\"nk\") is "
				+ (str3 == _str4.concat(str4_)));
		
		//--------------------------------------
		String str7 = "hel"+"lo";//这俩的拼接非创建了吧？
		String str7_ ="hello";
		System.out.println(str7_==str7);
		//--------------------------------------
		String str8 = "new";
		String str9 = "old";
		String str10 = str8+str9;
		System.out.println("newold"==str10);
		String str11 = "newold";
		System.out.println("newold"==str11);
	}

	/**
	 * 封装了下输出方法（换行）
	 * 
	 * @param params
	 */
	private static void println(Object params) {
		System.out.println(params);
	}

	/**
	 * 封装输出方法（不换行）
	 * 
	 * @param params
	 */
	private static void print(Object params) {
		System.out.print(params);
	}
}
``````````

近来太忙，先贴上，下个月估计可以多抽些时间。

