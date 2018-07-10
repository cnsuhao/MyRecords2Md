---
title: 对html元素进行监听
date: 2013-06-04 21:28:43
categories: "开发"
tags:
	- html

---

``````````
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> New Document </title>
  <meta name="Generator" content="EditPlus">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">

  <script type="text/javascript">
  <!--
	window.onload = function(){
		document.getElementById('north').onclick = function(e){
			if(e.target.nodeName=="BUTTON"
			||(e.target.nodeName=="INPUT"&&e.target.type=="button")){
			   var E = e.target;
			   alert(E.name+'----all');
			}
			return false;
		}
	}
  //-->
  </script>
 </head>

 <body>
  

  <div id='north'>
		<button name="yi">anniu</button>
		<input type='button' value='anNIU' name='er'/>
  </div>
  <div id='west'>
		<button name="san">anniu</button>
		<input type='button' value='anNIU' name='si'/>
  </div>
  <div id='center'>
        <button name="wu">anniu</button>
  </div>
 </body>
</html>
``````````


照旧上代码,原理javascript高级程序设计有。
