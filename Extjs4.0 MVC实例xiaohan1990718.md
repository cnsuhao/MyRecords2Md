---
title: Extjs4.0 MVC实例
date: 2011-12-21 17:54:04
categories: "开发"
tags:
	- Ext
	- js
	- MVC

---

extjs mvc 架构 ，有别于其他的MVC架构，EXTJS 有自己的定义

Model : 一个field 以及 他的data的集合。 Modles 知道如何持久化自己一般使用Stores 来表示数据以用于grids等component

View: 一种component的类型,grids,trees 以及panels等
Controller:用来放使得app工作的代码，例如 render views , instantiating Models 或者其他应用逻辑

Extjs 所有的MVC 程序都是用Application 类的实例开始的 。里面包含了这个应用的全局变量，如应用名，同时还包含了应用所需要的models,views以及controllers。 此外还包含了一个launch函数，这个函数会在加载的时候自动运行。例如
Ext.application(\{
name: 'AM',

appFolder: 'app',

controllers: \[
'Users'
\],

launch: function() \{
Ext.create('Ext.container.Viewport', \{
layout: 'fit',
items: \[
\{
xtype: 'panel',
title: 'Users',
html : 'List of users will go here'
\}
\]
\});
\}
\});

这里我们定义了一个应用名，系统会自动增加这样一个namespace ，此外，定义了相对的路径app.还告诉系统我们要用到Users这个controller.最终我们用launch定义了一个viewprot.
一般来说，一个系统会有多个controller ，一般是一个Model对用一个controller.


定义个controller：
controller是一个中介，主要是监听事件，特别是来自view的时间，以决定我们的系统如何继续运行。
Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',

init: function() \{
console.log('Initialized Users! This happens before the Application launch function is called');
\}
\});

注意，contorller 是在上述的application 实例中定义和调用。这里的init函数会在launch 之前调用 。然后，我们完善init 函数 。在init 函数里面这样写
Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',

init: function() \{
this.control(\{
'viewport > panel': \{
render: this.onPanelRendered
\}
\});
\},

onPanelRendered: function() \{
console.log('The panel was rendered');
\}
\});

这里使用了一个ComponentQuery 来，获得compontent ，简单的说这个这里使用了一个ComponentQuery就是使用一种类似CSS代码的方法选择component。这里的意思是选择所有是viewport的孩子panel的component.都调用onPanelRendered这个函数

下面我们定义view 首先定义个List 用来展示数据
Ext.define('AM.view.user.List' ,\{
extend: 'Ext.grid.Panel',
alias : 'widget.userlist',

title : 'All Users',

initComponent: function() \{
this.store = \{
fields: \['name', 'email'\],
data : \[
\{name: 'Ed', email: ['ed@sencha.com'][ed_sencha.com]\},
\{name: 'Tommy', email: ['tommy@sencha.com'][tommy_sencha.com]\}
\]
\};

this.columns = \[
\{header: 'Name', dataIndex: 'name', flex: 1\},
\{header: 'Email', dataIndex: 'email', flex: 1\}
\];

this.callParent(arguments);
\}
\});
这个view是一个最普通的component 。 一个grid panel的扩展 。这里的 alias 的作用就是让我们可以使用xtype 函数来创建Item。

然后就是使用这个list
首先实现修改user.js 里的controller 文件，来增加view的支持
Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',

views: \[
'user.List'
\],

init: ...

onPanelRendered: ...
\});
And then render it inside the main viewport by changing app.js to:

Ext.application(\{
name: 'AM',

controllers: \[
'Users'
\],

launch: function() \{
Ext.create('Ext.container.Viewport', \{
layout: 'fit',
items: \{
xtype: 'userlist'
\}
\});
\}
\});

注意，我们是需要在controller里面添加view这个数组，这样，当app 初始化 controllers数组以后，就会同时初始化这个controller里面的views 。这样，我们就能在viewport的item里面xtype了userlist。
这时候onrendered 函数同样会被调用，因为list实际上是一个gridpanel 。所以我们将controller修改如下

Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',

views: \[
'user.List'
\],

init: function() \{
this.control(\{
'userlist': \{
itemdblclick: this.editUser
\}
\});
\},

editUser: function(grid, record) \{
console.log('Double clicked on ' + record.get('name'));
\}
\});

注意，这里直接使用了 userlist 以简化，而对应的事件则是itedblclick 。也就是双击一行。


然后我们新建一个 Ext.define('AM.view.user.Edit', \{
extend: 'Ext.window.Window',
alias : 'widget.useredit',

title : 'Edit User',
layout: 'fit',
autoShow: true,

initComponent: function() \{
this.items = \[
\{
xtype: 'form',
items: \[
\{
xtype: 'textfield',
name : 'name',
fieldLabel: 'Name'
\},
\{
xtype: 'textfield',
name : 'email',
fieldLabel: 'Email'
\}
\]
\}
\];

this.buttons = \[
\{
text: 'Save',
action: 'save'
\},
\{
text: 'Cancel',
scope: this,
handler: this.close
\}
\];

this.callParent(arguments);
\}
\});

这个是一个简单的包含form的window 。以用来显示弹出的数据，包含两行 name 和 email
同样，我们扩展controller

Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',

views: \[
'user.List',
'user.Edit'
\],

init: ...

editUser: function(grid, record) \{
var view = Ext.widget('useredit');

view.down('form').loadRecord(record);
\}
\});

将eidt 增加到views数组里面,并增加一个editUser函数。注意这里使用了一个down 函数，这也是一个componentQuery函数。在Extjs4 里面，所有的component都有一个down方法，以用来快速找寻其孩子component

为了使用store 我们需要改两个地方，
Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',
stores: \[
'Users'
\],
...
\});
then we'll update app/view/user/List.js to simply reference the Store by id:

Ext.define('AM.view.user.List' ,\{
extend: 'Ext.grid.Panel',
alias : 'widget.userlist',

//we no longer define the Users store inline
store: 'Users',

...
\});

在这里将stores 放到 controller的 stores 数组里面，这样系统会自动装在这个store并且建立storeid，这样我们可以很容易的在view里面使用这个store 如 store: 'Users'。
在Extjs 里面提供给了一个强大的Model 类，用它我们可以很方便的编辑我们的内容。
Ext.define('AM.model.User', \{
extend: 'Ext.data.Model',
fields: \['name', 'email'\]
\});

//the Users controller will make sure that the User model is included on the page and available to our app
Ext.define('AM.controller.Users', \{
extend: 'Ext.app.Controller',
stores: \['Users'\],
models: \['User'\],
...
\});

// we now reference the Model instead of defining fields inline
Ext.define('AM.store.Users', \{
extend: 'Ext.data.Store',
model: 'User',

data: \[
\{name: 'Ed', email: ['ed@sencha.com'][ed_sencha.com]\},
\{name: 'Tommy', email: ['tommy@sencha.com'][tommy_sencha.com]\}
\]
\});
注意，是在sore里面添加 model的reference 。
下面我们在controller 里面监听save事件

Ext.define('AM.controller.Users', \{
init: function() \{
this.control(\{
'viewport > userlist': \{
itemdblclick: this.editUser
\},
'useredit button\[action=save\]': \{
click: this.updateUser
\}
\});
\},

updateUser: function(button) \{
console.log('clicked the Save button');
\}
\});

这也是一个 ComponentQuery selector 。首先找到useredit，然后找到button有save 这个action的button 这个action:save 的参数是我们在xtype useredit 的时候传进去的，而它给了我们一个快速找到这个button的方法 。
测试能够正确找到这个button以后，我们就可以完善方法了

updateUser: function(button) \{
var win = button.up('window'),
form = win.down('form'),
record = form.getRecord(),
values = form.getValues();

record.set(values);
win.close();
\}

这里我们使用up 来找到我们的父component，这样点击save 就可以将数据保存到 record 里面了。 下一步就是保存到server上

首先先将读取设成ajax 模式
Ext.define('AM.store.Users', \{
extend: 'Ext.data.Store',
model: 'User',
autoLoad: true,

proxy: \{
type: 'ajax',
url: 'data/users.json',
reader: \{
type: 'json',
root: 'users',
successProperty: 'success'
\}
\}
\});

这里，我们将date 换成了proxy ，这种格式可以让store 或者 model 保存或者读取数据。为了保存数据，我们需要修改一个这个sotre 的proxy
proxy: \{
type: 'ajax',
api: \{
read: 'data/users.json',
update: 'data/updateUsers.json'
\},
reader: \{
type: 'json',
root: 'users',
successProperty: 'success'
\}
\}


然后，在编辑完成以后让其同步一下来将数据上传
updateUser: function(button) \{
var win = button.up('window'),
form = win.down('form'),
record = form.getRecord(),
values = form.getValues();

record.set(values);
win.close();
this.getUsersStore().sync();
\}
转载：http://www.darren9.com/html/ExtJsxuexi/2011/0824/extjs4-mvc.html



[ed_sencha.com]: mailto:%27ed@sencha.com%27
[tommy_sencha.com]: mailto:%27tommy@sencha.com%27