---
title: List集合去重-- Java
date: 2014-03-15 11:27:55
categories: "开发"
tags:
	- Java

---

``````````java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;


/**
 * list集合工具类
 * @author hq
 *
 */
public class ListUtil {

	/**
	 * 去重
	 * @param list
	 * @return
	 */
	public static List<?> filterRepeat4List(List<?> list){
		List<Object> temp = new ArrayList<Object>();
		Map<Object,Boolean> tempMap = new HashMap<Object,Boolean>();
		for(Object obj : list){
			if(tempMap.get(obj) == null){
				tempMap.put(obj, true);
				temp.add(obj);
			}
		}
		return temp;
	}
	
	public static List<?> filterRepeat4List2(List<?> list){
		Map<Object,Boolean> tempMap = new HashMap<Object,Boolean>();
		Object obj = null;
		for(int i=0,len = list.size();i<len;i++){
			obj = list.get(i);
			if(tempMap.get(obj) == null){
				tempMap.put(obj, true);
			}else{
				list.remove(obj);
				len--;
				i--;
			}
		}
		return list;
	}
	
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		list.add("a");
		list.add("ae");
		list.add("ad");
		list.add("af");
		list.add("ab");
		list.add("acc");
		list.add("acb");
		list.add("aca");
		list.add("ac");
		list.add("123");
        list.add("123");
        list.add("ac");
		System.out.println(ListUtil.filterRepeat4List2(list));
	}

}
``````````

list去重, 随意写的.后来又添加了一个去重规则接口 供自定义规则适用。均在以上基础上扩展的。没难度，请自行研究。