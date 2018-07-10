---
title: Extjs4-数据的增删改
date: 2011-12-29 17:12:53
categories: "开发"
tags:
	- Ext
	- js

---

一般的项目中，Gird功能不是很复杂的话，都会使用window来实现数据的添加、修改功能，而本实例中，由于使用了动态grid功能，这样就使得再使用动态window来实现数据的添加和修改就会变的非常困难。

幸好，Grid有RowEditing，下面，我们就用RowEditing来实现数据的添加和修改功能。在开始之前，我们先回顾下上一章的一些重点内容：

1、给Menu增加了field属性，使得我们在数据库中的一些字段可以被当做是Menu的节点集合中的对象来调用。

2、给菜单表增加了两个字段，分别是store和columns。在app/store/文件夹下，我们新建了bastore.js文件，那么再数据库中对应的字段中，填写内容为bastore，在columns字段中，我们添加了内容为\{text:'序号',dataIndex:'ID'\},\{text:'公司名称',dataIndex:'kehu\_name'\}的数据。

最后，我们修改了Menu.js文件，使得Grid可以显示数据。

现在，我们修改columns中的数据为：

1.  \{text:'序号',dataIndex:'ID',width:50\},\{text:'公司名称',dataIndex:'kehu\_name',width:260,editor:\{allowBlank: false\}\},\{text:'备案号',dataIndex:'beianhao',width:140,editor:\{allowBlank: false\}\},\{text:'备案密码',dataIndex:'beianpass',width:100,editor:\{allowBlank: false\}\},\{text:'备案邮箱',dataIndex:'beianemail',width:160,editor:\{allowBlank: false\}\},\{text:'备案邮箱密码',dataIndex:'emailpass',width:120,editor:\{allowBlank: false\}\},\{text:'备案账号',dataIndex:'beianzh',width:160,editor:\{allowBlank: false\}\},\{text:'账号密码',dataIndex:'beianzhpa',width:120,editor:\{allowBlank: false\}\}

``````````
{text:'序号',dataIndex:'ID',width:50},{text:'公司名称',dataIndex:'kehu_name',width:260,editor:{allowBlank: false}},{text:'备案号',dataIndex:'beianhao',width:140,editor:{allowBlank: false}},{text:'备案密码',dataIndex:'beianpass',width:100,editor:{allowBlank: false}},{text:'备案邮箱',dataIndex:'beianemail',width:160,editor:{allowBlank: false}},{text:'备案邮箱密码',dataIndex:'emailpass',width:120,editor:{allowBlank: false}},{text:'备案账号',dataIndex:'beianzh',width:160,editor:{allowBlank: false}},{text:'账号密码',dataIndex:'beianzhpa',width:120,editor:{allowBlank: false}}
``````````

在这些数据中，将所有字段都列了出来，并且制定了editor中allowBlank的数据位flase,就是说，这些内容必须填写。

接下来，我们需要修改app/controller下的menu.js文件，增加一些功能，使其可以添加、删除信息。修改内容如下：

1.  if (record.get('leaf')) \{ 
2.  var panel = Ext.getCmp(record.get('id')); 
3.  if(!panel)\{ 
4.   Ext.require(\['Ext.grid.\*'\]); 
5.  var rowEditing = Ext.create('Ext.grid.plugin.RowEditing',\{ 
6.   clicksToMoveEditor: 1, 
7.   autoCancel: true
8.   \}); 
9.   panel = Ext.create("Ext.grid.Panel",\{ 
10.  store:record.get('stores'), 
11.  columns:record.get('columns'), 
12.  errorSummary:false, 
13.  title: record.get('text'), 
14.  id:record.get('text')+record.get('id'), 
15.  columnLines: true, 
16.  bodyPadding:0, 
17.  closable: true, 
18.  bbar: Ext.create('Ext.PagingToolbar', \{ 
19.  store: record.get('stores'), 
20.  displayInfo: true, 
21.  displayMsg: '显示 \{0\} - \{1\} 条，共计 \{2\} 条', 
22.  emptyMsg: "没有数据"
23.  \}), 
24.  dockedItems: \[\{ 
25.  xtype: 'toolbar', 
26.  items: \[\{ 
27.  text: '增加信息', 
28.  iconCls: 'icon-add', 
29.  handler: function()\{ 
30.  rowEditing.cancelEdit(); 
31. var panelStore = this.up("panel").getStore(); 
32. var panelModel = this.up("panel").getSelectionModel(); 
33.  panelStore.insert(0,panelModel); 
34.  rowEditing.startEdit(0, 0); 
35.  \} 
36.  \}, '-', \{ 
37.  itemId: 'delete', 
38.  text: '删除信息', 
39.  iconCls: 'icon-delete', 
40.  disabled: true, 
41.  handler: function()\{ 
42. var selection = panel.getView().getSelectionModel().getSelection()\[0\]; 
43. var panelStore = this.up("panel").getStore(); 
44. if (selection) \{ 
45.  panelStore.remove(selection); 
46.  \} 
47.  \} 
48.  \}, '-' ,\{ 
49.  text:'刷新数据', 
50.  iconCls:'icon-refresh', 
51.  handler:function()\{ 
52. var panelStore = this.up("panel").getStore(); 
53. var currPage = panelStore.currentPage; 
54.  panelStore.removeAll(); 
55.  panelStore.load(currPage); 
56.  \} 
57.  \}\] 
58.  \}\], 
59.  plugins: \[rowEditing\], 
60.  listeners: \{ 
61. 'selectionchange': function(view, records) \{ 
62.  panel.down('\#delete').setDisabled(!records.length); 
63.  \} 
64.  \} 
65.  \}) 
66. this.openTab(panel,record.get('id')); 
67.  \}else\{ 
68. var main = Ext.getCmp("content\_panel"); 
69.  main.setActiveTab(panel); 
70.  \} 
71.  \} 

``````````
if (record.get('leaf')) { 
            var panel = Ext.getCmp(record.get('id')); 
            if(!panel){
				Ext.require(['Ext.grid.*']);
				var rowEditing = Ext.create('Ext.grid.plugin.RowEditing',{
					clicksToMoveEditor: 1,
        			autoCancel: true
				});
				panel = Ext.create("Ext.grid.Panel",{
					store:record.get('stores'),
					columns:record.get('columns'),
					errorSummary:false,
					title: record.get('text'),
					id:record.get('text')+record.get('id'),
					columnLines: true,
					bodyPadding:0,
					closable: true,
					bbar: Ext.create('Ext.PagingToolbar', {
						store: record.get('stores'),
						displayInfo: true,
						displayMsg: '显示 {0} - {1} 条，共计 {2} 条',
						emptyMsg: "没有数据"
					}),
					dockedItems: [{
						xtype: 'toolbar',
						items: [{
							text: '增加信息',
							iconCls: 'icon-add',
							handler: function(){
								rowEditing.cancelEdit();
								var panelStore = this.up("panel").getStore();
								var panelModel = this.up("panel").getSelectionModel();
								panelStore.insert(0,panelModel);
								rowEditing.startEdit(0, 0);
							}
						}, '-', {
							itemId: 'delete',
							text: '删除信息',
							iconCls: 'icon-delete',
							disabled: true,
							handler: function(){
								var selection = panel.getView().getSelectionModel().getSelection()[0];
								var panelStore = this.up("panel").getStore();
								if (selection) {
									panelStore.remove(selection);
								}
							}
						}, '-' ,{
							text:'刷新数据',
							iconCls:'icon-refresh',
							handler:function(){
								var panelStore = this.up("panel").getStore();
								var currPage = panelStore.currentPage;
								panelStore.removeAll();
								panelStore.load(currPage);
							}
						}]
					}],
					plugins: [rowEditing],
					listeners: {
						'selectionchange': function(view, records) {
							panel.down('#delete').setDisabled(!records.length);
						}
					}
				})
				this.openTab(panel,record.get('id'));
            }else{
				var main = Ext.getCmp("content_panel");
				main.setActiveTab(panel); 
			}
        }
``````````

代码中，���用了插件rowEditing，增加了一个toolbar，上面添加了三个按钮，分别是添加信息，删除信息和刷新数据。添加信息的按钮，我们创建了一个handler，并且在其中获取了gird的Store和Model，并将其插入到grid的store中，删除信息的按钮中，设想这个按钮在没有选中任何行的时候，是不可用的，所以设置其disabled为true。还设置了handler，并且通过获取选择的行，将其从grid 的store中删除。并且，我们为grid增加了一个监听selectionchange，这样，我们就可以在选择中一条数据后，让删除信息的按钮变的可用。

目前，当点击添加按钮并添如信息后，只是存在于grid的store中，并没有将数据更新到数据库中，删除信息也是一样。接下来，需要修改app/store下的bastore.js，使其可以更新到数据中。

bastore.js：

1.  Ext.define('SMS.store.bastore', \{ 
2.   extend: 'Ext.data.Store', 
3.   requires: 'SMS.model.beianlistmodel', 
4.   model: 'SMS.model.beianlistmodel', 
5.   pageSize: 20, 
6.   remoteSort: true, 
7.   autoLoad:true, 
8.   proxy: \{ 
9.   type: 'ajax', 
10.  url: '/server/getbeian.asp', 
11.  reader: \{ 
12.  root: 'items', 
13.  totalProperty : 'total'
14.  \}, 
15.  simpleSortMode: true
16.  \}, 
17.  listeners:\{ 
18.  update:function(store,record)\{ 
19. var currPage = store.currentPage; 
20. //alert(record.get("ID"))  
21.  Ext.Ajax.request(\{ 
22.  url:'/server/getbeian.asp?action=save', 
23.  params:\{ 
24.  id : record.get("ID"), 
25.  kehu\_name:record.get("kehu\_name"), 
26.  beianhao:record.get("beianhao"), 
27.  beianpass:record.get("beianpass"), 
28.  beianemail:record.get("beianemail"), 
29.  emailpass:record.get("emailpass"), 
30.  beianzh:record.get("beianzh"), 
31.  beianzhpa:record.get("beianzhpa"), 
32.  \}, 
33.  success:function(response)\{ 
34.  store.removeAll(); 
35.  store.load(currPage); 
36.  \} 
37.  \}); 
38.  \}, 
39.  remove:function(store,record)\{ 
40. var currPage = store.currentPage; 
41. //alert(record.get("ID")) 
42.  Ext.Ajax.request(\{ 
43.  url:'/server/getbeian.asp?action=del', 
44.  params:\{ 
45.  id : record.get("ID") 
46.  \}, 
47.  success:function(response)\{ 
48.  store.removeAll(); 
49.  store.load(currPage); 
50.  \} 
51.  \}); 
52.  \} 
53.  \}, 
54.  sorters: \[\{ 
55.  property: 'ID', 
56.  direction: 'DESC'
57.  \}\] 
58. \});

``````````
Ext.define('SMS.store.bastore', {
    extend: 'Ext.data.Store',
    requires: 'SMS.model.beianlistmodel',
    model: 'SMS.model.beianlistmodel',
	pageSize: 20,
	remoteSort: true,
	autoLoad:true,
	proxy: {
		type: 'ajax',
		url: '/server/getbeian.asp',
		reader: {
			root: 'items',
			totalProperty  : 'total'
		},
		simpleSortMode: true
	},
	listeners:{
		update:function(store,record){
			var currPage = store.currentPage;
			//alert(record.get("ID"))
			Ext.Ajax.request({
				url:'/server/getbeian.asp?action=save',
				params:{
					id : record.get("ID"),
					kehu_name:record.get("kehu_name"),
					beianhao:record.get("beianhao"),
					beianpass:record.get("beianpass"),
					beianemail:record.get("beianemail"),
					emailpass:record.get("emailpass"),
					beianzh:record.get("beianzh"),
					beianzhpa:record.get("beianzhpa"),
				},
				success:function(response){
					store.removeAll();
					store.load(currPage);
				}
			});
		},
		remove:function(store,record){
			var currPage = store.currentPage;
			//alert(record.get("ID"))
			Ext.Ajax.request({
				url:'/server/getbeian.asp?action=del',
				params:{
					id : record.get("ID")
				},
				success:function(response){
					store.removeAll();
					store.load(currPage);
				}
			});
		}
	},
	sorters: [{
		property: 'ID',
		direction: 'DESC'
	}]
});
``````````

代码中，为store增加了两个监听，update和remove，并且将数据通过AJAX发送到服务端，在服务端进行处理，这里，只使用了update和remove。store中还有个add方法，此方法也是增加一条数据，������常理来说。这个方法才是增加数据，但是我使用了add方法之后，点击添加信息，就会添加一条空数据，索性就使用update方法，将id值也发送到服务端，在服务端进行处理，服务端处理流程是：接收id值，判断id值是否为空，如果为空，则新增数据，如果不为空，则更新数据。

至此，一个grid的功能全部完成了。而且本项目所有的功能，都是如此，这样，只要再数据库中插入相应的行，在app/store和app/model中建立相关的js就可以了。至于其他功能，就不在此一一例举了。

声明一点，这个开发笔记实施到最后，有一些东西已经脱离了MVC的范畴，而且也没想到六章内容就结束了这个项目。从文章一到六，只是起一个抛砖引玉的作用。由于Extjs4有很大的变动，所以任何基于Extjs4.x MVC的项目，都是摸着石头过河，一点一点积累起来的，并不是说我的这些���章起���了指导性的作用，而是实际开发过程中的一些体会和经验。所有项目，有很多种方法可以实现需求。