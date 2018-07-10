---
title: ID生成——简易方法
date: 2013-08-21 23:20:17
categories: "开发"
tags:
	- 技术
	- javascript

---

``````````
var NS = {};
NS.id = (function(){
   	var id = 100;
	var getId = function(){
        id+=1;
		return 'now'+id;
	}
	return getId;
})();
var id = NS.id();
var id2 = NS.id();
alert(id);
alert(id2);
``````````

睡觉.....今天完成了之前的一个想法 嘎嘎...坚决不在页面使用固定ID 是个好习惯~