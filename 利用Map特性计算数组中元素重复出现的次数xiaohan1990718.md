---
title: 利用Map特性计算数组中元素重复出现的次数
date: 2012-11-22 09:27:40
categories: "开发"
tags:
	- 技术
	- javascript

---

``````````
package mapPractice;

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
		String[] strArray = { "D", "BB", "C", "aAA", "C", "E", "E", "E" };
		// String[] strArray = new String[] { "yY", "Yy", "V", "v","z"
		// };//当然yY和Yy算不同字符，而y和Y却认为相同字符的话，还要做一些判断
	
		Map<String, Integer> map = getStrArrayMapResult(strArray);
		printMapResult(map);
	   
	}

	/**
	 * 通过map计算数组重复元素的个数主方法
	 * 
	 * @param strArray
	 *            字符串数组
	 * @return
	 */
	private static Map<String, Integer> getStrArrayMapResult(Object[] strArray) {
		Map<String, Integer> map = new HashMap<String, Integer>();
		for (int i = 0; i < strArray.length; i++) {
			// 如果为空 说明是该键是第一次put进map
			Object key = strArray[i];
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

	/**
	 * 遍历Map格式数据 使用特定的格式输出它们的个数 也可以根据具体情况 修改该方法 或者重用该方法
	 * 
	 * @param map
	 *            map格式数据
	 */
	private static void printMapResult(Map<String, Integer> map) {
		Set<String> set = map.keySet();// map的keySet()方法返回类型即Set集合，Set集合的特点是不重复，与map的键不重复是一致的。
		for (Iterator<String> it = set.iterator(); it.hasNext();) {
			Object key = it.next();
			key = changeKey(key);// --
			System.out.println(key + "的个数是：" + map.get(key));
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


上述代码将Object\[\]更改为String\[\]的话有些转换就不需要添加了。仅为了测试该例，因而写为static（静态）方法。




//断网缘故，更新暂断。





//如果计算字符串中的字符出现次数呢？ 你们想吧~