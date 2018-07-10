---
title: JS实现Map
date: 2013-01-31 14:38:29
categories: "开发"
tags:
	- JS

---

``````````
/**
 * @author haw_king
 * @returns {Map}
 */
var Map = function() {
    
};

Map.prototype = {
	/**
	 * 维护map对象的数组
	 * 
	 * @private
	 */
	elements : new Array(),
    /**
	 * 访问map对象内数组的接口方法
	 */
	strutsSet:function(){
		return this.elements;
	}, 
	/**
	 * 将指定的map复制到新map中
	 * 
	 * @param map
	 */
	putAll : function(map) {
       for(var i in map){
    	 this.put(i,map[i]);  
       }
       return this;
	},
	/**
	 * 向map中添加元素
	 * 
	 * @param key
	 * @param value
	 */
	put : function(key, value) {
		// 将对象维护到elements数组中，如果key相同，则覆盖原先的值
		var i = 0;
		if (this.containsKey(key, function(_i) {
			i = _i;
		})) {
			this.elements[i].value = value;
		}//else{}这里应该添加else{//这里是下面的代码}。勘误于此。
		var Struts = function(key, value) {
			this.key = key;
			this.value = value;
			this.getKey = function(){
				return this.key;
			};
			this.getValue=function(){
				return this.value;
			};
		};

		this.elements[this.elements.length] = new Struts(key, value);
	},
	/**
	 * 是否包含指定 key
	 * 
	 * @param {String}
	 *            key
	 * @param {Function}
	 *            callBack
	 * @returns {Boolean}
	 */
	containsKey : function(key, callBack) {
		for ( var i = 0, len = this.elements.length; i < len; i++) {
			if (this.elements[i].key === key) {
				if (callBack)
					callBack(i);
				return true;
			}
		}
		return false;
	},
	/**
	 * 是否包含指定的值
	 * @param value
	 * @returns {Boolean}
	 */
	containsValue : function(value) {
		for ( var i = 0, len = this.elements.length; i < len; i++) {
			if (this.elements[i].value === value) {
				return true;
			}
		}
		return false;
	},
	get : function(key) {
		for ( var i = 0, len = this.elements.length; i < len; i++) {
			if (this.elements[i].key === key) {
				return this.elements[i].value;
			}
		}
		return null;
	},
	remove : function(key) {
		 var bln = false;  
	        try {  
	            for (var i = 0; i < this.elements.length; i++) {  
	                if (this.elements[i].key == key) {  
	                    this.elements.splice(i, 1);  
	                    return true;  
	                }  
	            }  
	        } catch (e) {  
	            bln = false;  
	        }  
	        return bln;  
	},
	size : function() {
		return this.elements.length;
	},
	clear : function() {
        this.elements = [];
	},
	removeAll : function() {
		this.elements = [];
	},
	isEmpty:function(){
		return this.elements.length <= 0;
	}
};
``````````


在Java语言中，Map的实现是靠数组和链表完成的～但是源码看了下，不是很懂，就先看了下JS中的Map实现，稍微有点领悟，现将Map实现发表于此：




