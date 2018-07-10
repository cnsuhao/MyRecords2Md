---
title: Java中如何遍历Map呢？
date: 2012-11-09 21:43:07
categories: "开发"
tags:
	- Java

---

在Javascript中 遍历 key:value关系的对象时，有这样的方法：

``````````
var obj = {
      id:'123456789',
     name:'hank',
     age:22
};
for(var i=0,len = obj.length;i<len;i++){
     console.log(obj[i]);
}
``````````


或者遍历其属性和值

``````````
for(var i in obj){
    console.log("key="+i+"    value是"+obj[i]);
}
``````````


但在Java中遍历像Map这样的key:value关系的时候又怎么做呢？ 一下是4种遍历方法，并添加两种map<String,ArrayList<String>>的方法

``````````
package mapPractice;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.Map.Entry;
import java.util.Set;

/**
 * @Detail : 循环遍历Map的方法
 * @Author: 韩庆
 * @E-mail: IsaidIwillgoon@gmail.com
 * @Date: 2012-11-9
 * @Time: 下午8:48:01
 * 
 */
public class MapToTraverse {

	/**
	 * 用于得到键值-可以对键值做相应的处理
	 * 
	 * @return 处理键值关系后的字符
	 */
	public String getKeyAndValue(Map<String, String> map) {

		StringBuilder sb = new StringBuilder("结果是：{");
		// 利用hashmap中的entrySet()方法实现遍历
		Iterator<Entry<String, String>> it = map.entrySet().iterator();
		// 内部处理键值关系--以出键值关系为例
		while (it.hasNext()) {
			Map.Entry<String, String> entry = (Entry<String, String>) it.next();
			Object key = entry.getKey();
			Object value = entry.getValue();
			sb.append(key).append(":").append(value).append(",");// 因为Object隐式的调用toString()
																	// 我就不写了
		}

		return sb.substring(0, sb.length() - 1).concat("}");// 截取最后一个","并拼接“}”
	}

	public String getKeyAndValue2(Map<String, String> map) {
		StringBuilder sb = new StringBuilder("结果是：{");
		// for循环遍历 这是JDK1.5中的for-each
		for (Map.Entry<String, String> entry : map.entrySet()) {
			String key = entry.getKey();
			String value = entry.getValue();
			sb.append(key).append(":").append(value).append(",");
		}

		return sb.substring(0, sb.length() - 1).concat("}");
	}

	public String getKeyAndValue3(Map<String, String> map) {
		StringBuilder sb = new StringBuilder("结果是：{");
		// 利用 hashmap的keySet()方法
		for (Iterator<String> it = map.keySet().iterator(); it.hasNext();) {
			Object key = it.next();
			String value = map.get(key);// key隐式转换为Stirng
			sb.append(key).append(":").append(value).append(",");
		}

		return sb.substring(0, sb.length() - 1).concat("}");
	}

	public String getKeyAndValue4(Map<String, String> map) {
		StringBuilder sb = new StringBuilder("结果是：{");
        //这种方法老像 javascript中的for in 了  
		for (Object key : map.keySet()) {
			String value = map.get(key);
			sb.append(key).append(":").append(value).append(",");
		}

		return sb.substring(0, sb.length() - 1).concat("}");
	}

	/********************************************************
	 * Java中又是如何遍历Map<String,ArrayList> map = new HashMap<String,ArrayList<String>>()的呢？
	 *******************************************************/
	public String getMapForStringAndArrayList(Map<String,ArrayList<String>> map){
		
		//一下两个方法均在上述几个方法基础上 再次循环遍历其元素
		Set<String> keys = map.keySet();
		Iterator<String> it = keys.iterator();
		while(it.hasNext()){
			String key = it.next();
			ArrayList<String> arrayList = map.get(key);
			for(Object value : arrayList){
				System.out.println(value);
			}
			//这里可以处理键值关系
		}
		
		for(Map.Entry<String, ArrayList<String>> entry : map.entrySet()){
			String key = entry.getKey();
			List<String> lists = entry.getValue();
			for(String value : lists){
                System.out.println(key+":"+value);				
			}
			//这里可以处理键值关系
		}
		
		
		return null;
	}
	
	public static void main(String[] args) {
		Map<String, String> map = new HashMap<String, String>();
		map.put("id", "123456789");
		map.put("name", "hank");
		map.put("sex", "male");
		MapToTraverse mtt = new MapToTraverse();
		System.out.println(mtt.getKeyAndValue2(map));
	}
}
``````````


以上代码的方法参考了网上的一些源代码。贴在这里以供自己以后查看。 并上传java文件于资源中供下载。//以上纯手敲，错误难免 ![微笑][QJVY-NZEN-YNZ2.gif]。今天心情不是很好，不写了。


[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif