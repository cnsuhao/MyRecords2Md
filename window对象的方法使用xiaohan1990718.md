---
title: window对象的方法使用
date: 2012-12-19 22:41:38
categories: "开发"
tags:
	- 技术
	- javascript

---

![BVIY-VEQ2-QQQ3.jpg][]

利用setTimeout() confirm() clearTimeout() 实现定时提醒：

``````````
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> New Document </title>
  <meta name="Generator" content="EditPlus">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">

  <script type='text/javascript'>
  
        var _time,flag=false;
         
        function lock() {
            //页面刚加载时不提醒
  		    if(flag){
  		        //alert("锁屏操作code");
  				if(confirm('关闭网页么？')){
  				   window.close();
  				}
  		    }
            flag = true;
            window.clearTimeout(_time);//取消由 setTimeout() 方法设置的 timeout。
            _time = window.setTimeout("lock()", 1000 * 60*30);//半小时
			
        }
        //点击页面 
        document.onclick = function () {
            window.clearTimeout(_time);
        }; 
        //键盘响应
        document.onkeypress = function () {
            window.clearTimeout(_time);
        };            
        //页面加载时触发 
        window.onload = lock;
  
  
  </script>
 </head>

 <body>
  
 </body>
</html>
``````````


以上可以继续完善～


[BVIY-VEQ2-QQQ3.jpg]: /pro/os/crawler/BVIY-VEQ2-QQQ3.jpg