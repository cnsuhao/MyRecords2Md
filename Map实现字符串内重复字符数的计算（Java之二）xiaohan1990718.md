---
title: Map实现字符串内重复字符数的计算（Java之二）
date: 2012-11-25 07:25:01
categories: "开发"
tags:
	- Java

---

//我2了。。。不提也罢。

``````````
String word = "my name is hank,and you ?";
		char[] words = word.toCharArray();// 通过转换为字符的方法
		String[] newStr = new String[words.length];// String[]时初始化的长度要是words的长度
		for (int i = 0; i < words.length; i++) {
			char c = words[i];
			String charStr = null;
			switch (c) {
			case ' ':
				charStr = "空格";
				break;
			default:
				charStr = String.valueOf(c);
				break;
			}
			newStr[i] = charStr;
		}
``````````


这是利用String数字实现将字符串转换为字符数组后再次转换为String数组，这样一来就跟之前数组实现保持一致了。

总结下：

1、 将字符串-->字符数组-->字符串数组-->遍历数字 将数组内元素添加到map内，根据map特性判断添加value即可。（至于为何不使用字符数组---计算有误，具体原因我再查）；

2、 将字符串-->遍历截取每个字符-->存入字符串数组-->其后一致。

3、想将字符存入ArrayList()，但错误原因同1后括号内原因一样。---有空再研究。

4、这段时间有点累，再次更改上述代码：for循环内删除，由

``````````
newStr[i] = words[i]==' '?"空格":String.valueOf(words[i]);
``````````

代替。我真2，当时怎么想的呢。。。。。。。