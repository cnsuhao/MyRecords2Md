---
title: ExtJS中的面向对象设计,组件化编程思想
date: 2012-02-03 19:05:57
categories: "开发"
tags:
	- Ext
	- JS

---

/\*\*//\*
\* @author: Lilf
\* Description: ExtJS中的面向对象设计,组件化变成思想
\*/
/\*\*//\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*扩展VTypes类，增加年龄的验证\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
Ext.apply(Ext.form.VTypes, \{
"age": function(\_v)\{
if (/^\\d+$/.test(\_v)) \{
var intExp = parseInt(\_v);
if (intExp < 200)
return true;
\}
return false;
\},
ageText: "请输入正确的年龄格式，如：23 "
\});
/\*\*//\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*继承自FormPanel的表单组件,用来构件Window\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
PersonInfoFormPanel = Ext.extend(Ext.form.FormPanel, \{
constructor: function()\{
PersonInfoFormPanel.superclass.constructor.apply(this, \[\{
baseCls: "x-plain",
buttonAlign: "right",
labelWidth: 30 ,
defaultType: "textfield",
defaults: \{
anchor: "95%",
labelStyle: "text-align:right"
\},
items: \[\{
fieldLabel: "姓名",
name: "name"
\}, \{
fieldLabel: "年龄",
name: "age",
vtype: "age"//验证年龄（通过vtype类型来验证）
\}, \{
xtype: "combo",
mode: "local",//本地数据
readOnly: true,
fieldLabel: "性别",
displayField: "sex",//显示下拉框的内容
triggerAction: "all",//在选择时，显示所有的项
value: "男",//默认值
store: new Ext.data.SimpleStore(\{
fields: \["sex"\],
data: \[\["男"\], \["女"\]\]
\}),
name: "sex"//绑定字段
\}\]


\}\])
\},
//---以下为PersonInfoFormPanel类对外提供的方法---
getValues: function()\{
if (this.getForm().isValid())
return new Ext.data.Record(this.getForm().getValues());
else
throw new Error("验证没有通过");//自定义异常
\},
setValues: function(\_r)\{
this.getForm().loadRecord(\_r);
\},
reset: function()\{
this.getForm().reset();
\}


\});
/\*\*//\*\*\*\*\*\*\*\*\*\*\*\*\*继承自Window的基类,insertWindow与updateWindow都由此继承\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
baseWindow = Ext.extend(Ext.Window, \{
form: null,
constructor: function()\{
this.form = new PersonInfoFormPanel();//实例化PersonInfoFormPanel类
baseWindow.superclass.constructor.apply(this, \[\{
plain: true,
width: 350,
//title: "新增人员",
modal: true,
resizable: false,
closeAction: "hide",
defaults: \{
style: "padding:5px"
\},
items: this.form,
buttons: \[\{
text: "确 定",
handler: this.onSubmitClick,//提交事件调用
scope: this
\}, \{
text: "取 消",
handler: this.onCancelClick,//取消事件调用
scope: this

\}\]
\}\]);
//给insertWindow对象添加事件（事件冒泡）
this.addEvents("submit");
\},
//提交事件处理函数
onSubmitClick: function()\{
try \{
//发布事件
this.fireEvent("submit", this, this.form.getValues());//调用PersonInfoFormPanel类中自定义的方法getValues
this.close();

\}
catch (\_err) \{
Ext.Msg.alert("系统提示", \_err.description);//扑捉自定义错误或异常
\}
\},
//取消事件处理函数
onCancelClick: function()\{
this.close();
\},
//重置与隐藏事件处理函数
close: function()\{
this.form.reset();
this.hide();
\}

\});
/\*\*//\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*insertWindow类\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
insertWindow = Ext.extend(baseWindow, \{
title: "新增人员"
\});
/\*\*//\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*updateWindow类\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/
updateWindow = Ext.extend(baseWindow, \{
title: "修改人员",
load: function(\_r)\{
this.form.setValues(\_r);
\}
\});
/\*\*//\*\*\*\*\*\*\*根据上面组件创建新的GridPanel类,它就像我们根据不同的零件设计出来的汽车\*\*\*\*\*\*\*\*\*\*\*\*
\* ExtJs自定义PersonListGridPanel类
\* 该类继承自GridPanel\[使用Ext.extend(superClass,override Object)方法实现继承\],
\* 并override了该类的构造函�hu数
\* 构造函数内部继承自GridPanel的构造函数\[apply(this,arguments)实现继承\]
\* 该类实现了如何对外部公布一个事�件
\* 在构造函数中添加一个事件\[this.addEvents("事件名称"）\]
\* 然后使用this.fireEvent("事件名称",参数）来发布此事�件
\* 最后在客户端调用的时候来订阅该事�jian件
\*/
PersonListGridPanel = Ext.extend(Ext.grid.GridPanel, \{
\_window: null,
\_updateWin: null,
constructor: function(\_url)\{
this.\_window = new insertWindow();//insertWindow对象引用
this.\_updateWin = new updateWindow();//updateWindow对象引用
PersonListGridPanel.superclass.constructor.apply(this, \[\{
renderTo: Ext.getBody(),
width: 550,
height:200,
frame: true,
layout: "form",
//工具栏
tbar: \[\{
text: "add",
handler: function()\{
this.\_window.show();
\},
scope: this
\}, "-", \{
text: "update",
handler: function()\{
this.\_updateWin.show();
try \{
this.\_updateWin.load(this.getSelected());
\}


catch (\_err) \{
Ext.Msg.alert("系统提示", \_err.description);
this.\_updateWin.close();
\}
\},
scope: this
\}, "-", \{
text: "delete",
handler: this.onRemovePerson,
scope: this
\}\],
enableColumnMove: false,
//列模板
columns: \[\{
header: "Name",
menuDisabled: true,
dataIndex: "name"
\}, \{
header: "Age",
menuDisabled: true,
dataIndex: "age"
\}, \{
header: "Sex",
menuDisabled: true,
dataIndex: "sex"
\}\],
//数据源
store: new Ext.data.JsonStore(\{
autoLoad: true,
url: \_url,
fields: \["name", "age", "sex"\]
\}),
//选中模板
selModel: new Ext.grid.RowSelectionModel(\{
singleSelect: true,
listeners: \{
"rowselect": \{
fn: this.onRowSelected,
scope: this
\}
\}
\})

\}\]);
//添加事件
this.addEvents("rowselect");
//事件订阅
this.\_window.on("submit", this.onInsertWinSubmit, this);
this.\_updateWin.on("submit", this.onUpdateWinSubmit, this);
\},
//----- 以下为自定义方法---------
//获得选中的记录
getSelected: function()\{
var \_sm = this.getSelectionModel();
if (\_sm.getCount() == 0)
throw new Error("你没有选中任何记录,请选择一条记录后重试");
return \_sm.getSelected();
\},
//插入一条记录
insert: function(\_r)\{
this.getStore().add(\_r);
\},
//更新选中的记录
update: function(\_r)\{
try \{
var \_rs = this.getSelected();
var \_data = \_rs.data;
for (var \_i in \_data) \{
\_rs.set(\_i, \_r.get(\_i));
\};
\_rs.commit();
\}
catch (\_err) \{

\}

\},
//删除选中的记录
remove: function()\{
try \{
var \_rs = this.getSelected();
Ext.Msg.confirm("系统提示", "你确定删除吗?", function(\_btn)\{
if (\_btn == "yes")
this.getStore().remove(\_rs);
\}, this);
\}
catch (\_err) \{
Ext.Msg.alert("系统提示", \_err.description);
\}
\},
//-------以下为自定义事件处理函数------------
//添加事件
onInsertWinSubmit: function(\_win, \_r)\{
this.insert(\_r);
\},
//修改事件
onUpdateWinSubmit: function(\_win, \_r)\{
this.update(\_r);
\},
//删除事件
onRemovePerson: function()\{
this.remove();
\},
//选中事件
onRowSelected: function(\_sel, \_index, \_r)\{
this.fireEvent("rowselect", \_r);//发布事件
\}

\})

转自于：http://www.cnblogs.com/xnxylf/archive/2009/04/02/1428403.html
