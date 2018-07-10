---
title: google地图的简易封装（一）
date: 2013-11-26 22:06:37
categories: "开发"
tags:
	- 地图

---

以下经测试通过：


``````````
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> New Document </title>
  <meta name="Generator" content="EditPlus">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">
  <script type="text/javascript" src="http://maps.googleapis.com/maps/api/js?v=3.8&sensor=false&language=zh-CN"></script>
     <script type="text/javascript">
    var apply = function(base,expand){//用于copy属性
	    for(var key in expand){
		   base[key] = expand[key];
		}
	};
	var MyMap = function(id,mapOptions){
		//用于得到类型
        var _getMapTypeId = function(type){
			var mapTypeMap = {
				"NORMAL":google.maps.MapTypeId.ROADMAP,//地图
				"HYBRID":google.maps.MapTypeId.HYBRID,//卫星混合图
				"TERRAIN":google.maps.MapTypeId.TERRAIN//地形图
			};
			return mapTypeMap[type]||google.maps.MapTypeId.ROADMAP;
		},ggListenersMap = {zoom_changed:"zoom_changed",click : "click",mousemove:"mousemove"};//控制监听事件范围,方便兼容控制

		var baseOption = {//内置初始数据
		    center: new google.maps.LatLng(32.397, 120.144),//维度 Latitude  经度Longitude  
			zoom: 7,
			mapTypeId: _getMapTypeId("NORMAL")
		};
        apply(baseOption,mapOptions||{});
        var mapCanvas = document.getElementById(id);
		var map = new google.maps.Map(mapCanvas,baseOption);
           
        
		/*this.setOptions = function(mapOptions){
		    map.setOptions(mapOptions);//因想与百度兼容,暂不了解百度Map,不开放
		}*/
        //设置zoom大小,因对百度Map不了解,范围暂不控制
		this.setZoom = function(zoom){
		    map.setZoom(zoom);
		};

		this.getZoom = function(){
		    return map.getZoom();
		};
        /**
		 * 设置中心点 
		 * 经纬度
		 */
		this.setCenter = function(latitude,longitude){
			 map.setCenter(new google.maps.LatLng(latitude,longitude));
		}

        //添加监听  内部会扩展
		this.addListeners = function(listenersMap){
		   var scope = listenersMap['scope']||this;
		   for(var key in listenersMap){
			      if(!ggListenersMap[key]){continue;} 
                  //var arr = [this];
				  google.maps.event.addListener(map,ggListenersMap[key],function(){
						   /*for(var i=0,len = arguments.length;i<len;i++){
							  arr = arr.concat(arguments[i]);
						   }
						   listenersMap[key].apply(scope||this,arr);这里注释待研究下百度Map确定*/
                          listenersMap[key].call(scope);
				 });
            }
	    }
		
		//...etc 扩展
    };
  </script>
  <script type="text/javascript">
      window.onload = function(){
	     var myMap = new MyMap('myMapCanvas');
         myMap.addListeners({
		     click:function(){
			    alert(myMap.getZoom());
			 }
		 });
	  }
  </script>
 </head>
 <body>
    <div id="myMapCanvas" style="width: 100%; height: 400px"></div>
 </body>

</html>
``````````

做了层简易的封装,测试通过,最终想达到的目标：兼容百度map。 对它俩的特殊性进行区分。路还长,这是我做ext封装map时抽出的部分.方便另一种思路的扩展~

