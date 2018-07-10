---
title: 起名～Java程序源码。
date: 2013-01-12 16:09:15
categories: "开发"
tags:
	- Java

---

``````````
package array;

import java.util.Random;

public class Name {

	private static String[] x = { "赵", "钱", "孙", "李", "周", "吴", "郑", "王", "韩",
			"高", "郭", "欧阳", "张", "慕容" };// 维护“姓”数组
	private static String[] nvm = { "娜", "宁", "晓", "婷", "玲", "楠", "惠", "梅",
			"静", "怡" };// 维护女“名”数组
	private static String[] nanm = { "庆", "亮", "红", "伟", "磊", "刚", "飞", "志",
			"军", "成", "俊", "辉", "清", "斌", "冰" };// 维护男“名”数组

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		for (int i = 0; i < 1000; i++) {
			System.out.println(getName());
		}
	}

	/**
	 * 得到名字
	 * 
	 * @return
	 */
	public static String getName() {
		Random r = new Random();
		StringBuilder name = new StringBuilder();
		int forloop = r.nextInt(2) + 1;// 确定名字个数
		int randomX = r.nextInt(x.length);// 确定姓
		int xb = r.nextInt(2);// 用于判断性别 0<=xb<2;每次确定一个性别
		for (int i = 0; i < forloop; i++) {
			String[] xbm = null;
			if (xb % 2 == 0) {// 偶数为男名 
				xbm = nanm;
			} else {
				xbm = nvm;
			}
			int randomM = r.nextInt(xbm.length);
			//如果今后添加双字名 则需要进一步判断  1、name已经存在的数 2、xbm现在的数
			name.append(xbm[randomM]);
		}
		return x[randomX] + name;
	}

}
``````````
