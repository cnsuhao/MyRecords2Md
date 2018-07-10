---
title: 还是利用Map特性，计算字符串里字符出现的次数（Javascript版-Obj特性）
date: 2012-11-24 12:06:18
categories: "开发"
tags:
	- Java

---

``````````
资源下载地址：http://download.csdn.net/download/xiaohan1990718/4804826

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
    <title>js版，利用obj特性，计算重复字符元素的个数</title>
    <script type='text/javascript'>
        //稍作修缮，将其整成页面输入的
       function checkTextArea(){

           //var str = '我w20-30%长度  ,我w0ooo%%';
           var str = document.getElementById('myTextArea').value;
           if(str==null||''==str){
               document.getElementById('myDiv').innerHTML='您未输入任何值！';
               return;
           }
           var obj={},i=0;
           for(var j=0;j<str.length;j++){
               //依次截取字符串的字符 存入数组，其他的继续循环
               var textStr = str.substring(i,i+1)==' '?'空格':str.substring(i,i+1);//回车暂未搞定,回车的取值是什么？
               if(!obj[textStr]){//obj[textStr]==undefined
                   obj[textStr] = 1;
               }else{
                   obj[textStr] = obj[textStr]+1;
               }
               i++;//自增 以便下次循环时字符串的依次截取顺序
           }
           //思考：如果还要计算该字符出现的位置呢？（输出入：a字符在字符串“abca”中出现的位置是 1、4）
           var result = '';
           for(var k in obj){
               //document.writeln('字符：'+k+' 在字符串"'+str+'"中出现的次数:'+obj[k]+'</br>');
               //document.writeln(k+"："+obj[k]+'</br>');
               result+= k+':'+obj[k]+'</br>';
           }
           document.getElementById('myDiv').innerHTML=result;
       }
         </script>
</head>
<body>
<textarea rows="3" cols="100" id='myTextArea' onblur="checkTextArea()"></textarea>
<!--这里回车会计算有误，因为暂时我还没办法取得回车的判断方法。当计算值为“：数值”的时候，说明它是回车-->
<div id='myDiv'>输入值后，文本失去焦点即可查看字符重复元素计算结果。</div>
</body>
</html>
``````````

以上是利用Javascript的对象特性计算字符串里字符重复出现的次数。

//稍后发表Java版的。Java版的其他两个办法我暂时还不知道为何错，发表的这个版本是与Javascript版方法类似的。

//以上代码经测试通过。![微笑][QJVY-NZEN-YNZ2.gif]



[QJVY-NZEN-YNZ2.gif]: /pro/os/crawler/QJVY-NZEN-YNZ2.gif