---
title: Map特性，计算字符串内重复元素的个数（Java版）--之一
date: 2012-11-24 12:10:50
categories: "开发"
tags:
	- Java

---

``````````
package mapPractice;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

/**
 * 
 * @Detail :用Map来计算数组中出现相同字符的数量
 * @Author: 韩庆（QQ:haw_king@foxmail.com）
 * @E-mail: IsaidIwillgoon@gmail.com
 * @Date: 2012-11-18
 * @Time: 下午11:03:22
 * 
 */
public class MapTest1 {

	/**
	 * @param args
	 *            也可以利用args输入字符 来计算
	 */
	public static void main(String[] args) {
		String[] strArray = { "m", "y", " ", "n", "a", "m", "e", " ", "i", "s",
				" ", "h", "a", "n", "k", ",", "a", "n", "d", " ", "y", "o",
				"u", " ", "?" };
		// String[] strArray = new String[] { "yY", "Yy", "V", "v","z"
		// };//当然yY和Yy算不同字符，而y和Y却认为相同字符的话，还要做一些判断

		 Map<Object, Integer> map = getStrArrayMapResult(strArray);
		 printMapResult(map);//计算正常

		// 字符计算的错误。。。。。。why??

		// String word = "my name is hank,and you ?";
		// char[] words = word.toCharArray();
		// ArrayList newStr = new ArrayList();
		// for (int i = 0; i < words.length; i++) {
		// char c = words[i];
		// switch (c) {
		// case ' ':
		// c = '-';
		// break;
		// default:
		// break;
		// }
		// newStr.add(c);
		// }
		// System.out.println(newStr);
		// Map<Object, Integer> charMap =
		// getStrArrayMapResultForCharArray(newStr);
		// printMapResult(charMap);

		// String word = "my name is hank,and you ?";
		// char[] words = word.toCharArray();//通过转换为字符的方法
		// 此方法暂无法搞定。。。。使用法：1、字符的key:value方法结果是错的。但是key的问题我搞不清楚；2、这种方式字符初始化的搞不定
		// String strNew = words.toString();
		// String[] newStr = new String[]
		// {strNew};//String[]时初始化的长度要是words的长度，为何不行？
		// for (int i = 0; i < words.length; i++) {
		// char c = words[i];
		// String charStr = null;
		// switch (c) {
		// case ' ':
		// charStr = "空格";
		// break;
		// default:
		// charStr = String.valueOf(c);
		// break;
		// }
		// newStr[i] = new
		// String(charStr);//转进arrayList内也是计算错误，但是不知道为何key被覆盖是仍然判断Object.getKey()为空呢？
		// }
		//
		// Map<Object, Integer> newMap = getStrArrayMapResult(newStr);
		// printMapResult(newMap);
``````````

``````````
//加粗部分是Java版字符串计算字符重复个数的方法。 其他部分是我测试使用的。可不用看。
		String str = "my name is hank . and you ?";
		int i = 0;
		Map<String, Integer> newMap = new HashMap<String, Integer>();
		for (int j = 0; j < str.length(); j++) {
			String newStr = str.substring(i, i + 1);//分隔字符串，每次取一个字符。
			newStr = newStr.equals(" ") ? "空格" : newStr;
			if (newMap.get(newStr) == null) {
				newMap.put(newStr, 1);
			} else {
				newMap.put(newStr, newMap.get(newStr) + 1);
			}
			i++;
		}
		Set set = newMap.keySet();
		// map.entrySet();
		for (Iterator it = set.iterator(); it.hasNext();) {
			Object key = it.next();
			System.out.println(key + ":" + newMap.get(key));
		}

	}

	/**
	 * 通过map计算数组重复元素的个数主方法
	 * 
	 * @param strArray
	 *            字符串数组
	 * @return
	 */
	private static Map<Object, Integer> getStrArrayMapResult(Object[] strArray) {
		Map<Object, Integer> map = new HashMap<Object, Integer>();
		for (int i = 0; i < strArray.length; i++) {
			// 如果为空 说明是该键是第一次put进map
			Object key = strArray[i].equals(" ")?"空格":strArray[i];
			key = changeKey(key);// --
			if (map.get(key) == null) {
				map.put(key.toString(), new Integer(1));
			} else {
				// 不为空则覆盖其键，并将其值递增加1 用以计算其出现的次数
				map.put(key.toString(), map.get(key) + 1);
			}
		}
		return map;
	}

	private static Map<Object, Integer> getStrArrayMapResultForCharArray(
			ArrayList strChar) {
		Map<Object, Integer> map = new HashMap<Object, Integer>();

		for (int i = 0; i < strChar.size(); i++) {
			Object key = strChar.get(i);
			if (map.get(key) == null) {
				map.put(key.toString(), new Integer(1));
			} else {
				map.put(key.toString(), map.get(key) + 1);
			}
		}
		return map;
	}

	/**
	 * 遍历Map格式数据 使用特定的格式输出它们的个数 也可以根据具体情况 修改该方法 或者重用该方法
	 * 
	 * @param map
	 *            map格式数据
	 */
	private static void printMapResult(Map<Object, Integer> map) {
		Set<Object> set = map.keySet();// map的keySet()方法返回类型即Set集合，Set集合的特点是不重复，与map的键不重复是一致的。
		for (Iterator<Object> it = set.iterator(); it.hasNext();) {
			Object key = it.next();
			key = changeKey(key);// --
			System.out.println(key + "出现的个数是：" + map.get(key));
		}
	}

	/**
	 * 转换key字符的方法--此方法抽出来是为了更灵活
	 * 
	 * @param key
	 *            需转换的key字符
	 * @return
	 */
	private static Object changeKey(Object key) {
		key = ((String) key).toLowerCase();// 将键均转换为小写 这行可以根据需要做转换。
		return key;
	}
}
``````````

//以上部分不当之处，暂时未修改。 ![微笑][QJVY-NZEN-YNZ2.gif]


[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif