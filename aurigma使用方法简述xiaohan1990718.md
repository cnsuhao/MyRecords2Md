---
title: aurigma使用方法简述
date: 2014-01-15 22:45:46
categories: "开发"
tags:
	- aurigma

---

aurigma做什么的,请google,能干什么,请到www.aurigma.com。

本博文讲解其与jsp技术结合的用法,在8.x之前的版本中,如7.0 其技术API里并不支持jsp技术。对php和asp有很多的涉及。

到官网上下载其windows下的安装包后，直接运行安装即可。

1、7.x中 在安装的过程中即需要其licenseKey 请到官方申请个帐号,再申请1个月试用的ｋｅｙ即可。当然如果是你仅仅在你本机使用学习和测试的话，在８.ｘ 版本中不需要帐号的申请之类的，安装后直接到安装目录下可以找到样例demo以及API.

2、这里吐槽下,8.X中的API 貌似没有更新,我在使用API的过程中发现有些方法是不存在的。

3、![YIE7-JUV7-JFMJ.jpg][]

4、我使用的是其html5-flash中的jsp版本.导入其内部给的样例：

![QI6B-NYFV-IIIV.jpg][]


5、直接运行：localhost:8080/Upload/BasicDemo/index.jsp

![6FJA-BV2Q-26JQ.jpg][]


6、根据实际需求选择相应的demo进行修改，这里我选择的是files upload demo 并做了改造。汉化包在/Upload/WebRoot/Scripts/aurigma.imageuploaderflash.zh\_cn\_localization.js。

![7JN3-EJUY-BF6F.jpg][]


ps:汉化包引入后竟然没作用 我手动copy到组件内重写的。具体原因不明.查看汉化结果实际对象是被修改汉化了的,但是页面上汉化失败.

``````````
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE>
<html>
  <head>
    <base href="<%=basePath%>">
    <title>上传样例</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">

    <link rel="stylesheet" type="text/css" href="Scripts/css/aurigma.htmluploader.control.css" />
	<script src="Scripts/aurigma.imageuploaderflash.min.js" type="text/javascript"></script>
    <script src="Scripts/aurigma.htmluploader.control.js" type="text/javascript"></script>
	<script src="Scripts/aurigma.imageuploaderflash.zh_cn_localization.js" type="text/javascript"></script>
  </head>
  <body>
	 <script type="text/javascript">
	   window.onload = function(){
		   var show = document.getElementById('show');
		   document.getElementById('showOrHide').onclick = function(){
			   if(show.style.display == 'none'){
				   show.style.display = 'block'
			   }else{
				   show.style.display = 'none'
			   }
		   }
		   var uploader = $au.imageUploaderFlash({
			   "addFilesProgressDialog": {
			        "text": "正在将文件添加到上传列表..."
			    },
			    "commonDialog": {
			        "cancelButtonText": "取消",
			        "okButtonText": "确定"
			    },
			    "descriptionEditor": {
			        "cancelButtonText": "取消",
			        "saveButtonText": "保存"
			    },
			    "imagePreviewWindow": {
			        "closePreviewTooltip": "单击以关闭"
			    },
			    "messages": {
			        "cannotReadFile": "无法读取文件： {0}。",
			        "dimensionsTooLarge": "“{0}”的尺寸太大，无法添加该文件。 允许的最大图像尺寸是 {1} x {2} 像素。",
			        "dimensionsTooSmall": "“{0}”的尺寸太小，无法添加该文件。 允许的最小图像尺寸是 {1} x {2} 像素。",
			        "fileNameNotAllowed": "不能选择文件 “{0}”。 此文件有不允许的名词。",
					"fileSizeTooSmall": "“{0}”的尺寸太小，无法添加。 允许的最小尺寸为 {1}。",
			        "filesNotAdded": "由于站点限制，有 {0} 个文件未添加。",
			        "maxFileCountExceeded": "并非所有文件均已添加。 最多允许您上传 {0} 个文件。",
			        "maxFileSizeExceeded": "“{0}”的尺寸太大，无法添加。 允许的最大尺寸为 {1}。",
			        "maxTotalFileSizeExceeded": "并非所有文件均已添加。 超过了最大文件总大小 ({0})。",
			        "previewNotAvailable": "预览不可用",
			        "tooFewFiles": "要启动上传，至少应选中 {0} 个文件。"
			    },
			    "paneItem": {
			        "descriptionEditorIconTooltip": "编辑说明",
			        "imageTooltip": "{0}\n{1}、{3}、\n修改时间： {2}",
			        "itemTooltip": "{0}\n{1}、\n修改时间： {2}",
			        "removalIconTooltip": "删除",
			        "rotationIconTooltip": "旋转"
			    },
			    "statusPane": {
			        "dataUploadedText": "已上传数据： {0} / {1}",
			        "filesPreparedText": "已准备文件： {0} / {1}",
			        "filesToUploadText": "<font color=\"#7a7a7a\">文件总数：</font> {0}",
			        "filesUploadedText": "已完成文件： {0} / {1}",
			        "noFilesToUploadText": "<font color=\"#7a7a7a\">没有可上传的文件</font>",
			        "preparingText": "正在准备...",
			        "sendingText": "正在上传..."
			    },
			    "topPane": {
			        "addFilesHyperlinkText": "添加更多文件",
			        "clearAllHyperlinkText": "删除所有文件",
			        "orText": "或者",
			        "titleText": "要上传的文件",
			        "viewComboBox": ["平铺", "缩略图", "图标"],
			        "viewComboBoxText": "更改视图："
			    },
			    "uploadErrorDialog": {
			        "hideDetailsButtonText": "隐藏详细信息",
			        "message": "并非所有文件均已成功上传。 如果您看到此消息，请与网络主管联系。",
			        "showDetailsButtonText": "显示详细信息",
			        "title": "上传错误 "
			    },
			    /*"uploadPane": {
			        "addFilesButtonText": "+ 添加更多文件"
			    },*/
			    uploadButtonText:'上传',
			    cancelUploadButtonText:'取消上传',
			    id: 'Uploader1',
			    type: 'flash|html',
			    licenseKey: 'XXX',//请申请试用key.
			    width: '500px',
			    height: '180px',
			    enableRotation: false,
			    folderPane: {
    	            viewMode: 'Tiles'
			    },
			    uploadPane: {
			    	viewMode: 'Icons',
			    	"addFilesButtonText": "+ 添加更多文件"
			    },
			    converters: [
					{ mode: '*.*=SourceFile' }
			    ], 
			    uploadSettings: {
					actionUrl: 'servlet/Upload',
					autoRecoveryMaxAttemptCount: 1,//10 自动接收的最大临时数量?
	                autoRecoveryTimeout: 2000,
	                filesPerPackage: 1,//每个包里的文件数?
	                chunkSize: 104857600,
	                connectionTimeout: 3000//10s
			    },   
			    flashControl: {
					codeBase: 'Scripts/aurigma.imageuploaderflash.swf',
					bgColor: '#c0c0c0'
			    }, 
			    events: {
					beforeUpload: function() {
			    	    //if(this.files().count()>1){
			    	    	//alert('上传文件数 被限制为1 .请删除多余文件.');
			    	    	//return false;
			    	    //}
					},
					afterUpload:function(){//上传之后均需要重置面板--改控件自身支持
						console.log(arguments);
					    //show.style.display = 'none'
					},
					error:function(a,httpStatus,c,servserData){
						 alert('错误:' + servserData);
					},
					preRender:function(){//渲染前处理
						//uploader.writeHtml();//getHtml();
					}
			    }
			});
		   show.innerHTML = uploader.getHtml();
		    //uploader.writeHtml();
	   }
            </script>
            <div id="code">code</div>
            <div id="name">name</div>
            <input type="button" id = "showOrHide" value="上传组件"/>
            <div id="show"></div>
  </body>
</html>
``````````


7、关于actionUrl 我做了改动,方便放置到servlet中。

![U7BI-RUBV-6VUQ.jpg][]


8、Upload.js代码,除了添加以下代码外,均在upload.jsp中可找到.

``````````
response.setContentType("text/html");
		response.setCharacterEncoding("utf-8");
		
		PrintWriter out = response.getWriter();
		
		PageContext pageContext = JspFactory.getDefaultFactory().getPageContext(this, request, response, null, true, 20*1024, true);
		BaseGallery gallery = new BaseGallery(pageContext);
	    
	    // Create a factory for disk-based file items
	    FileItemFactory factory = new DiskFileItemFactory(1024*50, new File(gallery.getTempFilesAbsolutePath()));
``````````


9、效果:

![6RUE-EVR2-U7F2.jpg][]


10、该软件为收费,价格在200-1800美刀之间.已购买正版,可深入学习其知识,今后再研究其内部这些bug问题.（如更改视图下拉被连续点击多次即卡掉,文件上传一定数量或大小浏览器挂掉等）









[YIE7-JUV7-JFMJ.jpg]: /pro/os/crawler/YIE7-JUV7-JFMJ.jpg
[QI6B-NYFV-IIIV.jpg]: /pro/os/crawler/QI6B-NYFV-IIIV.jpg
[6FJA-BV2Q-26JQ.jpg]: /pro/os/crawler/6FJA-BV2Q-26JQ.jpg
[7JN3-EJUY-BF6F.jpg]: /pro/os/crawler/7JN3-EJUY-BF6F.jpg
[U7BI-RUBV-6VUQ.jpg]: /pro/os/crawler/U7BI-RUBV-6VUQ.jpg
[6RUE-EVR2-U7F2.jpg]: /pro/os/crawler/6RUE-EVR2-U7F2.jpg