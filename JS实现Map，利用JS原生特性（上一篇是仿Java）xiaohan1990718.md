---
title: JS实现Map，利用JS原生特性（上一篇是仿Java）
date: 2013-01-31 17:53:13
categories: "开发"
tags:
	- Java
	- JS

---

``````````
var Map = function(){
   var data = {};
   var length = 0;
   this.put = function(key,value){
      if(!this.containsKey(key)){
	     length++;
	  }
      data[key] = value;
   };
   this.size = function(){
     return length;
   };
   this.get = function(key){
     return data[key];
   };
   this.containsKey = function(key){
      if(data[key]){
	     return true;
	  }
	  return false;
   };
   this.putAll = function(map){
      for(var i in map){
	      this.put(i,map[i]);
	  }
   };
   this.entrySet = function(){
      return data;
   }
}
var map = new Map();
map.put('key1','value1');
map.put('key2','value2');
map.put('key2','value3');
map.putAll({'key3':'value333'});

var entry = map.entrySet();
for(var i in entry){
   alert(i+":"+entry[i]);
}
``````````
