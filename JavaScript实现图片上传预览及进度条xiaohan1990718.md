---
title: JavaScript实现图片上传预览及进度条
date: 2014-01-12 00:50:44
categories: "开发"
tags:
	- Java

---

最近在做图片上传的时候，由于产品设计的比较fashion，上网找了比较久还没有现成的，因此自己做了一个，实现的功能如下：

1：去除浏览器<input type=”file”>默认的样式；

2：图片从本地选择后，立即预览图片；

3：使用上传可以查看上传进度（按照上传的百分比做成进度条）；

先看界面效果图：

![JavaScript实现图片上传预览及进度条界面效果图][JavaScript]

功能效果图如下：

![JavaScript实现图片上传预览及进度条功能效果图][JavaScript 1]

首先是去除浏览器默认上传图片框，这个不是设置的css，再者<input type=”file”>具有安全性的限制，应该不是那么好操作的。这里使用的方法很简单，设置<input>为隐藏，再写个<div>,这个<div>标签的onclick事件触发<input>的onclick事件：

``````````
<input id="localpic1" type="file" style="display:none">
<div id="showpic1" onclick="document.getElementById('localpic1').click();" data="点击添加海报">点击添加海报</div>
``````````

这个功能比较简单，下面来看如何实现图片的预览。

图片的预览利用了<input type=”file”>的onchange事件，当检测到图片替换的时候，显示替换后的图片，我这边是直接插入的<img>标签，下面是布局：

``````````
 <li>
     <font>海报</font>
     <div id="showpic1" onclick="document.getElementById('localpic1').click();" data="点击添加海报">
          点击添加海报
     </div>
     <div id="showpic1Name"></div>
     <div id="showpic1Size"></div>
     <div>
        <a id="showpic1Up" style="display:none" href="#" onclick="uploadFile('pic1')">上传</a>
        <a id="showpic1Me" style="display:none" href="#">上传中</a>
        <a id="showpic1De" href="#" style="display:none" onclick="deletePic('pic1')">删除</a>
     </div>
</li>
``````````

onchange事件：获取图片的地址（value），好吧，这个万恶的各种浏览器。然后获取图片的信息予以显示，这个相对还是比较简单的，聪明的小伙伴应该一下子就明白了。

``````````
<input id="locallogo" type="file" onchange="previewImage(this,'showlogo');">

    /*
     *图片的操作
    */
    function previewImage(fileObj,divPreviewId){
        var allowExtention=".jpg,.bmp,.gif,.png";//允许上传文件的后缀名document.getElementById("hfAllowPicSuffix").value;
        var extention=fileObj.value.substring(fileObj.value.lastIndexOf(".")+1).toLowerCase();            
        var browserVersion= window.navigator.userAgent.toUpperCase();
        if(allowExtention.indexOf(extention)>-1){ 
            if(fileObj.files){//兼容chrome、火狐7+、360浏览器5.5+等，应该也兼容ie10，HTML5实现预览
                if(window.FileReader){
                    var reader = new FileReader(); 
                    reader.onload = function(e){
                        document.getElementById(divPreviewId).innerHTML="<img src='"+e.target.result+"'>";
                    }  
                    reader.readAsDataURL(fileObj.files[0]);
                }else if(browserVersion.indexOf("SAFARI")>-1){
                    alert("不支持Safari浏览器6.0以下版本的图片预览!");
                }
            }else if (browserVersion.indexOf("MSIE")>-1){//ie、360低版本预览
                if(browserVersion.indexOf("MSIE 6")>-1){//ie6
                    document.getElementById(divPreviewId).innerHTML="<img src='"+fileObj.value+"'>";
                }else{//ie[7-9]
                    fileObj.select();
                    if(browserVersion.indexOf("MSIE 9")>-1)
                        fileObj.blur();//不加上document.selection.createRange().text在ie9会拒绝访问
                    var newPreview =document.getElementById(divPreviewId+"New");
                    if(newPreview==null){
                        newPreview =document.createElement("div");
                        newPreview.setAttribute("id",divPreviewId+"New");
                        newPreview.style.width = "150px";
                        newPreview.style.height = "150px";
                        newPreview.style.border="solid 1px #d2e2e2";
                    }
                    newPreview.style.filter="progid:DXImageTransform.Microsoft.AlphaImageLoader(sizingMethod='scale',src='" + document.selection.createRange().text + "')";                            
                    var tempDivPreview=document.getElementById(divPreviewId);
                    tempDivPreview.parentNode.insertBefore(newPreview,tempDivPreview);
                    tempDivPreview.style.display="none";                    
                }
            }else if(browserVersion.indexOf("FIREFOX")>-1){//firefox
                var firefoxVersion= parseFloat(browserVersion.toLowerCase().match(/firefox\/([\d.]+)/)[1]);
                if(firefoxVersion<7){//firefox7以下版本
                    document.getElementById(divPreviewId).innerHTML="<img src='"+fileObj.files[0].getAsDataURL()+"'>";
                }else{//firefox7.0+ 
                    document.getElementById(divPreviewId).innerHTML="<img src='"+window.URL.createObjectURL(fileObj.files[0])+"'>";
                }
            }else{
                document.getElementById(divPreviewId).innerHTML="<img src='"+fileObj.value+"'>";
            }         
        }else{
            alert("仅支持"+allowExtention+"为后缀名的文件!");
            fileObj.value="";//清空选中文件
            if(browserVersion.indexOf("MSIE")>-1){                        
                fileObj.select();
                document.selection.clear();
            }                
            fileObj.outerHTML=fileObj.outerHTML;
        }
        document.getElementById(divPreviewId).style.border="";
        document.getElementById(divPreviewId+"Up").style.display="block";
        document.getElementById(divPreviewId+"De").style.display="block";
        showFileInfo(fileObj.files[0],divPreviewId);
    }
    //获取图片的大小、名称予以显示，这里还可以显示图片的文件类型
    function showFileInfo(file,divPreviewId) {
        var fileName = file.name;
        var file_typename = fileName.substring(fileName.lastIndexOf('.'), fileName.length);
        if (file) {
            var fileSize = 0;
            if (file.size > 1024 * 1024){
                fileSize = (Math.round(file.size * 100 / (1024 * 1024)) / 100).toString() + 'MB';
            }else{
                fileSize = (Math.round(file.size * 100 / 1024) / 100).toString() + 'KB';
            }
            document.getElementById(divPreviewId+"Name").innerHTML = '文件名: ' + file.name;
            document.getElementById(divPreviewId+"Size").innerHTML = '图片大小: ' + fileSize;
            document.getElementById(divPreviewId+"Name").style.display="block";
            document.getElementById(divPreviewId+"Size").style.display="block";
        }
    }
``````````

最后一个进入条问题比较难以解决，你要监听图片上传了多少。我觉得大部分人在这里会选择插件，那样的话就不用自己管了，但是如果有兴趣，继续往下看吧。

当我用<form>解决不了，用ajax解决不了的时候（我看ajax有人好像说能上传文件，这个处理下应该是可以的），就想到了XMLHttpRequest()。

``````````
function uploadFile(fileId) {
        document.getElementById("show"+fileId+"Up").style.display="none";
        document.getElementById("show"+fileId+"Me").style.display="block";
        var fd = new FormData();
        fd.append("file", document.getElementById("local"+fileId).files[0]);
        var xhr = new XMLHttpRequest();
        //上传中设置上传的百分比
        xhr.upload.addEventListener("progress", function(evt){
            if (evt.lengthComputable) {
                var percentComplete = Math.round(evt.loaded * 100 / evt.total);
                document.getElementById("show"+fileId+"Me").innerHTML = '上传中'+percentComplete+"%";
            }else {
                document.getElementById("show"+fileId+"Me").innerHTML = '无法计算';
            }
        }, false);
        //请求完成后执行的操作
        xhr.addEventListener("load", function(evt){
            var message = evt.target.responseText,
                obj = eval("("+message+")");
            if(obj.status == 1){
                $("#"+fileId).val(obj.picName);
                document.getElementById("show"+fileId+"Me").innerHTML = "已上传";
                  alert("上传成功!");
            }else{
                alert(obj.message);
            }

        }, false);
        //请求error
        xhr.addEventListener("error", uploadFailed, false);
        //请求中断
        xhr.addEventListener("abort", uploadCanceled, false);
        //发送请求
        xhr.open("POST", "${ctx}/manage/file/File/uploadPic.htm");
        xhr.send(fd);
    }

    function uploadFailed(evt) {
        alert("上传出错.");
    }

    function uploadCanceled(evt) {
        alert("上传已由用户或浏览器取消删除连接.");
    }
``````````

OK，最后一个问题搞定了，在Firefox，IE9，Chrome下测试都没有问题，版本应该都不高，其他的就不管了，哈哈。

\--转自:http://www.xprogrammer.com/966.html


[JavaScript]: /pro/os/crawler/AJQZ-ZEZ2-ENZB.jpg
[JavaScript 1]: /pro/os/crawler/EJAU-Q2FV-JZQE.jpg