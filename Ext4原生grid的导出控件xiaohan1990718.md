---
title: Ext4原生grid的导出控件
date: 2013-12-18 15:56:56
categories: "开发"
tags:
	- Ext

---

前言 ：导出建议在后端进行,避免一些兼容性问题。

本文借鉴网络上的一个js包,在此基础上稍微完善了些，并添加了即将扩展和完善的东西，今后会逐步完善它。

使用样例:

``````````
<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>Excel导出(前端)</title>
    
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<link rel="stylesheet" type="text/css" href="plugs/ext4.2/resources/css/ext-all.css">
	<script type="text/javascript" src="plugs/ext4.2/ext-all-dev.js"></script>
	<script type="text/javascript" src="public/util/Exporter2Excel-all.js"></script>
    <script type="text/javascript">
         Ext.onReady(function(){
        	 
	        	var store =  Ext.create('Ext.data.Store', {
							    fields:['id','name', 'email', 'phone'],
							    data:{'items':[
							        { id:'0', 'name': 'Lisa',  "email":"lisa@simpsons.com",  "phone":"555-111-1224"  },
							        { id:'1', 'name': 'Bart',  "email":"bart@simpsons.com",  "phone":"555-222-1234" },
							        { id:'2',  'name': 'Homer', "email":"home@simpsons.com",  "phone":"555-222-1244"  },
							        { id:'3',  'name': 'Lisa',  "email":"lisa@simpsons.com",  "phone":"555-111-1224"  },
							        { id:'4',  'name': 'Bart',  "email":"bart@simpsons.com",  "phone":"555-222-1234" },
							        { id:'5',  'name': 'Homer', "email":"home@simpsons.com",  "phone":"555-222-1244"  },
							        { id:'6',  'name': 'Lisa',  "email":"lisa@simpsons.com",  "phone":"555-111-1224"  },
							        { id:'7',  'name': 'Bart',  "email":"bart@simpsons.com",  "phone":"555-222-1234" },
							        { id:'8',  'name': 'Homer', "email":"home@simpsons.com",  "phone":"555-222-1244"  },
							        { id:'9',  'name': 'Lisa',  "email":"lisa@simpsons.com",  "phone":"555-111-1224"  },
							        { id:'10',  'name': 'Bart',  "email":"bart@simpsons.com",  "phone":"555-222-1234" },
							        { id:'11',  'name': 'Homer', "email":"home@simpsons.com",  "phone":"555-222-1244"  },
							        { id:'12',  'name': 'Marge', "email":"marge@simpsons.com", "phone":"555-222-1254"  }
							    ]},
							    proxy: {
							        type: 'memory',
							        reader: {
							            type: 'json',
							            root: 'items'
							        }
							    }
							});
				var nameStore = Ext.create('Ext.data.Store',{
					            fields:['name', 'id'],
							    data:[{'id': 'Lisa',  "name":"汉字111111"},
							        {'id': 'Bart',  "name":"汉字222222"},
							        {'id': 'Homer', "name":"汉字333333"},
							        {'id': 'Marge',  "name":"汉字555555"}]
								});
				var nameCom = Ext.create('Ext.form.field.ComboBox',{
					            store:nameStore,
					            queryMode: 'local',
							    displayField: 'name',
							    valueField: 'id'
				});
				var gridPanel = Ext.create('Ext.grid.Panel', {
					            border:false,
							    store: store,
							    columns: [
							    	{text:'ID',dataIndex:'id',hidden:true},
							        { text: '姓名',  dataIndex: 'name' ,renderer:function(value){
							            nameCom.setValue(value);
							            return nameCom.getRawValue();
								 }},
							        { text: '邮件', dataIndex: 'email', flex: 1 },
							        { text: '手机号码', dataIndex: 'phone' }
							    ]
				});
        	 
        	 var pagePanel = Ext.create('Ext.panel.Panel',{
        		 title: 'Simpsons',
        		 height:'100%',
        		 width:'100%',
        		 tbar:[{
        			 text:'导出',
        			 handler:function(){
			              window.location = getExcelUrl.getExcelUrl(gridPanel, "表格导出");
        			 }
        		 }],
        		 layout:'fit',
        		 border:false,
        		 items:[gridPanel],
        		 renderTo: Ext.getBody()
        	 });
        	 
        	 
        	 
         });
    
    
    
    
    
    
       
    <%--
          
    --%>
    
    </script>
  </head>
  
  <body>
   
  </body>
</html>
``````````


界面效果:

![RVYR-AA7R-UZN3.jpg][]


实际导出效果:

![2URJ-7BZF-ZF6R.jpg][]


资源下载：http://download.csdn.net/detail/xiaohan1990718/6735001 样例即上面例子。



[RVYR-AA7R-UZN3.jpg]: /pro/os/crawler/RVYR-AA7R-UZN3.jpg
[2URJ-7BZF-ZF6R.jpg]: /pro/os/crawler/2URJ-7BZF-ZF6R.jpg