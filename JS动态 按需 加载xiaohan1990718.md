---
title: JS动态 按需 加载
date: 2013-11-25 23:22:22
categories: "开发"
tags:
	- JS

---

现在想做这么一件事:

在创建类之前,动态的添加其资源,且添加一次。

在页面load前,所谓的动态按需加载有：

document.wrieln(js标签和路径); document.appendChild(js对象); 等等...并非我想要.


``````````
HQ.loadJSFile = function(className){//单一文件加载,可多个扩展
      var xhr = null,obj = null,status = null;
      
      if(loadClassManager[className]){
      return loadClassManager[className];
      } 
      var fileName = className.replace(/\./g,"/"),js = doc.createElement('SCRIPT'),head = doc.getElementsByTagName('head')[0];
 js.src = fileName+".js?"+(+new Date());
 js.charset = "utf-8";
 js.type = "text/javascript";
     if(head){head.appendChild(js);}else{throw '该页面必须包含head标签元素!!';}
      
      try {
      if (typeof XMLHttpRequest != 'undefined') {//Ajax请求
      xhr = new XMLHttpRequest();
      } else {
      xhr = new ActiveXObject('Microsoft.XMLHTTP');
      }
             xhr.open('GET', fileName+".js?dt="+(+new Date()), false);//同步
             xhr.send(null);
             status = xhr.status;
             if ((status >= 200 && status < 300) || (status === 304)){//FireFox下通过 
               var classes = className.split(".");
     var hq = null;
     try{
      for(var i=0,len = classes.length;i<len;i++){
      if(i==0){
      hq = HQ;
      }else{
      hq = hq[classes[i]];
      }
      }
    }catch(e){
       throw e.message;
       }
              obj = new hq();
          loadClassManager[className] = obj; 
             }
         } catch (e) {
          throw e.message;
         }
         return obj;
    };
``````````

以上在火狐下验证,时而成功,其他浏览器均失败.

思路是这样的：

将文件以ajax同步的形式加载进来,同时先将对应的资源添加到页面.最后动态的new一个对象 里面是为了测试,并且返回了这个对象。

记录与此,希望有相同需求的将思路分享下,我在寻去解决办法。

