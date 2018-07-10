---
title: Javascript Function()扩展
date: 2013-05-12 14:08:22
categories: "开发"
tags:
	- Java

---

1、概述

在Javascript中，function非常灵活且功能强大。我们可以通过new Function(‘’,’’) 、eval()来等动态构建函数，这在别的语言（Java）中很难做到的。但这里不是介绍Function的特性，而是考量Function函数的扩展，比较Mootools2,Prototype6,Ext02中对Function函数的扩展，分析在构建新的类库时，怎样扩展Function函数。

Function.prototype有两个特有用，使用频率极高的两个函数，一是function.apply(thisArg\[, argsArray\]);，二是fun.call(thisArg\[, arg1\[, arg2\[, ...\]\]\]);。两个函数的作用都是一样的，更改函数作用域并执行函数。使函数的this指向thisArg。也可以它变换成调用执行thisArg.fun.（argsArray或arg1\[, arg2\[, ...\]\]）。Call与Apply不同的地方就是一个采用数组传入参数，一个采用Arguments的形式。

除了这种更改函数作用域并执行函数的功能，我们在操作函数时，还需要别的什么功能呢？试想一下Java,C++,C\#等语言的中函数。我们会想到函数回调，函数委托，函数包装。这对应着模式设计中的观察者模式，代理模式，装饰模式。这样在JavaScript的函数中又如何处理呢？

说到回调函数，在JavaScript中，我们也并不陌生，我们在网页上经常会写如：node.onclick=myFn,为该Node的click事件注册一个处理函数。这个处理函数就是回调函数。它要按一定的格式来写。比如它默认把onclick的事件做为参数传入到myFn中。也就是说myFn函数要这样写：function(e){…};在函数中就可以调用系统传进来e,根据e做相关的处理（IE中不兼容）。但是在Javascript中，对于函数的参数没有其它的语言那样的要求（个数，类型等）。不管你的函数有几个参数，它只传入一个参数。当然啊，对于Javascript的其它的回调就不一定是一个参数了。

这样看来，为某个函数创建回调函数的意义不大，但是在代码采用createCallBack的方式比直接写函数名有更好的可理解和可读性。假如我的一个函数如下：funciton(e,x1,x2,x3){},采用createCallBack的方式，我们可以预先设定x1,x2,x3的值。这样一来，我们就可以做到为多种回调写一个处理函数。只在在注册创建回调时设定值来区分。

函数委托，就是本来是我这个函数的工作，我要委托别的函数来实现。但是调用的却是我这个函数。在JavaScript中，改变了函数的执行作用域，其实就改变函数的归属。本来属于A对象，现在可以就变成了B对象。函数委托或代理其实就只要改变这个执行作用域就可以了。与Apply不同的，改变执行作用域的时候不执行该函数，来推迟到下一次的调用时才执行。那不就是把这个函数代理包装另外一个函数，实际上还是执行这个改变了作用域的函数。

函数包装，就把原来的函数包装成功能更强大的函数。在JavaScript中，要包装这个函数，那首先得把这个函数传到包装的函数中来，然后才进行相关的加强装饰操作。因为函数是有执行作用域的，对于被包装的函数，我们的要求肯定是和包装后的函数的执行作用域是一样的。这样一来，才相当于调用那个被包装的函数,同时又加强了功能。

上面三种都是对函数对于一些改变成为另外一个函数，那么函数是不是还需要有一些操作呢，比如我想在调用这个函数时1s后执行呢，比如我们每隔执行这个函数呢？还有在执行这个函数之前，想执行别的函数，或之后执行别的函数？定时或间隔执行在操作网页的样式渐变有很大的作用。
采用函数名表式：createCallBack, createDelegate, createWrap, delay, periodical, before,after，在扩展Function时，我们大约就只需要这几种操作。那么这几种操作在Mootools2,Prototype6,Ext02中是否全都实现了呢，又是如何实现的呢？


分析与比较


在Mootools2,Prototype6,Ext02都实现了callback,degelate。不过感觉起来差强人意。先
1说callback吧：
Ext中：

```javascript
	createCallback : function(){ 
		var args = arguments; 
		var method = this; 
		return function(){ 
			return method.apply(window, args); 
		}; 
	} 
```


这个callback，没有处理闭包函数传进来的参数，比如在事件回调中，会传入event进来，而这样却处理不了。唯一能处理的是闭包函数无传进来的参数的回调。而函数的执行作用域竟然是window,理应该是调用该闭包函数的this。
ProtoType中：

```javascript
  bindAsEventListener: function() { 
	  	var __method = this, args = $A(arguments), object = args.shift(); 
	  	return function(event) { 
		  return __method.apply(object, \[event || window.event\].concat(args)); 
		} 
  } 
 
```

这个也是完成callback的功能，不过我它只能完成事件的callback功能，如果要用的callback，就捉襟见肘了。在这里，函数中的执行作用域也只是事先传来的object，觉得object不好，为什么不给一个默认的作用域为闭包函数的调用对象呢？可以改成object||this。\[event || window.event\]改成$A(arguments)可以了。
Moo中：

```javascript
  bindWithEvent: function(bind, args){
  	return this.create({bind: bind, event: true, arguments: args}); 
  }, 
  create: function(options){
	  var self = this; 
	  options = options || {}; 
	  return function(event){
		  var args = options.arguments; 
		  args = (args != undefined) ? $splat(args) : Array.slice(arguments, (options.event) ? 1 : 0); 
		 if (options.event) args = \[event || window.event\].extend(args); 
		 var returns = function(){
		 	return self.apply(options.bind || null, args); 
		 }; 
		 if (options.delay) return setTimeout(returns, options.delay); 
		 if (options.periodical) return setInterval(returns, options.periodical); 
		 if (options.attempt) return $try(returns); 
	 	return returns(); 
	 }; 
 }, 
```

发现ProtoType中一模一样的处理。只不过是集中在create（）中了。
从上面三者可以看出来，它们都没有真正地实现callback函数。如下实现：

```javascript
  createCallBack : function(obj, args, appendArgs){
  	  var method = this; 
	  return function() {
	  		var callArgs = args || arguments; 
		   if(appendArgs === true){
			  callArgs = Array.prototype.slice.call(arguments, 0); 
			  callArgs = callArgs.concat(args); 
		   }else if(typeof appendArgs == "number"){
		  	  callArgs = Array.prototype.slice.call(arguments, 0); 
		  	  var applyArgs = [appendArgs, 0].concat(args); 		  Array.prototype.splice.apply(callArgs, applyArgs); 
		   } 
	 	  return method.apply(obj ||this|| window, callArgs); 
	 }; 
 }, 
```

这样一来，我们在创建Callback时，可以动态指定参数的位置，预设参数。突破了callback仅仅只是事件中的。

2 Delegate


Ext中：

```javascript
  createDelegate : function(obj, args, appendArgs){
  	  var method = this; 
	  return function() {
		  var callArgs = args || arguments; 
		  if(appendArgs === true){
			  callArgs = Array.prototype.slice.call(arguments, 0); 
		  	  callArgs = callArgs.concat(args); 
		  }elseif(typeof appendArgs == "number"){
		  	callArgs = Array.prototype.slice.call(arguments, 0); var applyArgs = [appendArgs, 0\].concat(args); 
		  	Array.prototype.splice.apply(callArgs, applyArgs); } 
		 	return method.apply(obj || window, callArgs); 
		 }; 
 }, 

```

感觉到一点不好的地方就是函数的作用域obj || window改成obj ||this|| window使用默认的时候指调用对象的本身。这样没有什么不好。

Prototype中：


```javascript

  bind: function() {
	  if (arguments.length < 2 && arguments[0] === undefined) return this; 
	  var __method = this, args = $A(arguments), object = args.shift(); 
	  return function() {
	  	return __method.apply(object, args.concat($A(arguments))); 
	  } 
  } 
```

比Ext中的差远了。不能设定参数的位置。而且用bind命名，给人误解。myFn.bind(this,xx,yy),给人感觉和myFn.call(this,xx,yy)是一样的，修改执行作用域并执行。那里可以看得出来返回是函数。

Moo中：

```javascript
  bind: function(bind, args){
  	return this.create({bind: bind, arguments: args}); 
  },  
```

其调用create(),和Prototype中bind()差不多。

3包装

只有prototype中有：


```javascript
  wrap: function(wrapper) {
	  var __method = this; 
	  return function() {
	  	return wrapper.apply(this,[__method.bind(this)].concat($A(arguments))); 
	  } 
  }, 
```
一不能指定作用域，二不能用函数预设参数。

修改如下：

```javascript

  createWrap:function(wrapper,scope,args, appendArgs){
  	  var _method = this; 
	  return function() {
	  	var allArgs = Array.prototype.slice.call(arguments, 0); 
		  return wrapper.apply(scope||this,[_method.createDelegate(scope||this, args, appendArgs)].concat(allArgs)); 
	   } 
  }, 
```
现在可以传入作用域，还可以指该被包装的函数的预设参数。同时能改变参数的位置。


```javascript
  var a = {
	b: {
	  c: function () { } 
  	}, 
	f: function () {
	  	alert(this==a.b); 
	} 
 }; 
 
 a.b.c = a.f.wrap(function (F) {
 	F(); 
 }); 
```

a.b.c();
最好是采用a.f.wrap(function (F) {F();})这样的形式来书写wrapper;

Delay吧;

Ext中：

```javascript

defer : function(millis, obj, args, appendArgs){
	var fn = this.createDelegate(obj, args, appendArgs);
	if(millis){
		return setTimeout(fn, millis);
	}
	fn();
	return 0;
},
```

Protype中：

```javascript

delay: function() {
	var __method = this, args = $A(arguments), timeout = args.shift() * 1000;
	return window.setTimeout(function() {
		return __method.apply(__method, args);
	}, timeout);
},
```

Moo

```javascript

delay: function(delay, bind, args){
	return this.create({delay: delay, bind: bind, arguments: args})();
},


periodical: function(interval, bind, args){
	return this.create({periodical: interval, bind: bind, arguments: args})();
},

periodical : function(millis, obj, args, appendArgs){
	var fn = this.createDelegate(obj, args, appendArgs);
	if(millis){
		return setInterval(fn, millis);
	}
	fn();
	return 0;
},

```


form : http://blog.csdn.net/lcj8/article/details/3980447?reload


[icon_copy.gif 1]: http://jljlpch.javaeye.com/blog/221646#
[view plain]: http://blog.csdn.net/lcj8/article/details/3980447#