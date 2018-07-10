---
title: 关于Ext基础部分
date: 2011-11-11 01:46:55
categories: "开发"
tags:
	- Ext

---

从昨天起，项目经理让我这刚进公司的小喽啰学习Ext，并要求一些功能实现，然后的然后我就学了这两天，这算是我职业生涯的开始吧，从今天起，我就断断续续纪录它吧。

 **Ext**

什么是Ext?

.ExtJs的简称，是一个强大的js类库*Ext*简介 *Ext*是一个强大的js类库，以前是基于YAHOO-UI,现在已经完全独立了， 主要包括data,widget,form,grid,dd,menu,其中最强大的应该算grid了，编程思想是基于面向对象编程(oop),扩展性相当的主要包括data,widget,form,grid,dd,menu,其中最强大的应该算grid了,编程思想是基于面向对象编程(oop),扩展性相当的好.可以自己写扩展.自己定义命名空间.web应用可能感觉太大.不过您可以根据需要按需加载您想要的类库就可以了.

　　主要包括三个大的文件ext-all.css,ext-base.js,ext-all.js(包括所有的类库,您可以根据需要进行删减.官方网站提供这一接口),在引用ext类库的时候.这三个文件必不可少.

　　它提供了丰富的，非常漂亮的外观体验，成为众多界面层开发人员的追捧！其核心的组件基本覆盖了我们构建富客户端的常用的组件。

有关这个项目的代码，等我这周完成后跟上。

2011年11月11日 小韩记于郑州。

[ExtJS之面向对象编程基本知识][ExtJS]

1：支持命名空**间**

 < script type = " text/javascript " > 
// 定义一个命名空间
Ext.namespace("Ext.wentao");
// 在命名空间上定义一个类
Ext.wentao.helloworld = Ext.emptyFn;
// 创建一个类的实例
new Ext.wentao.helloworld(); 
  </ script >

 其中 

 Ext.wentao.helloworld  =  Ext.emptyFn;

等价于 

 Ext.wentao.helloworld  =  function() \{\} ;


**2：支持类实例属性**

 < script type = " text/javascript " > 
Ext.namespace("Ext.wentao"); // 自定义一个命名空间
Ext.wentao.Person = Ext.emptyFn; // 在命名空间上自定义一个类 

// 为自定义的类 增加一个 name 属性，并赋值
Ext.apply(Ext.wentao.Person.prototype, \{
name : "刘文涛"
\});

var \_person = new Ext.wentao.Person();// 实例化 自定义类
alert(\_person.name);
\_person.name = "张三";// 修改类name属性
alert(\_person.name);

 </ script >


**3：支持类实例方法**

 < script type = " text/javascript " >
  Ext.namespace("Ext.wentao"); // 自定义一个命名空间
Ext.wentao.Person = Ext.emptyFn; // 在命名空间上自定义一个类 

// 演示类实例方法
Ext.apply(Ext.wentao.Person.prototype, \{
name : "刘文涛",
sex : "男",
print : function() \{
alert(String.format("姓名:\{0\},性别:\{1\}", this.name, this.sex));
\}
\});

var \_person = new Ext.wentao.Person();// 实例化 自定义类
\_person.print();


 </ script >


**4：支持类静态方法**

![None.gif][] < script type = " text/javascript " > 


Ext.namespace("Ext.wentao"); // 自定义一个命名空间
Ext.wentao.Person = Ext.emptyFn; // 在命名空间上自定义一个类

// 演示类实例方法
Ext.apply(Ext.wentao.Person.prototype, \{
name : "刘文涛",
sex : "男",
print : function() \{
alert(String.format("姓名:\{0\},性别:\{1\}", this.name, this.sex));
\}
\});

// 演示 类静态方法
Ext.wentao.Person.print = function(\_name, \_sex) \{
var \_person = new Ext.wentao.Person();
\_person.name = \_name;
\_person.sex = \_sex;
\_person.print(); // 此处调用类 实例方法，上面print是类 静态方法
\};

Ext.wentao.Person.print("张三", "女"); // 调用类 静态方法

 </ script >


**5：支持构造方法**

![None.gif][] < script type = " text/javascript " > 
 Ext.namespace("Ext.wentao"); //自定义一个命名空间
![None.gif][]
![None.gif][]//构造方法
 Ext.wentao.Person = function(\_cfg)\{
Ext.apply(this,\_cfg);
\};
![None.gif][]
![None.gif][]//演示类实例方法
 Ext.apply(Ext.wentao.Person.prototype, \{
print:function()\{
alert(String.format("姓名:\{0\},性别:\{1\}",this.name,this.sex));
\}
\});
![None.gif][]
![None.gif][]//演示 类静态方法
 Ext.wentao.Person.print = function(\_name,\_sex)\{
var \_person =new Ext.wentao.Person(\{name:\_name,sex:\_sex\});
\_person.print(); //此处调用类 实例方法，上面print是类 静态方法
 \};
![None.gif][]
![None.gif][] Ext.wentao.Person.print("张三","女"); //调用类 静态方法
  </ script >


**6：支持类继承**

![None.gif][] < script type = " text/javascript " > 


Ext.namespace("Ext.wentao"); // 自定义一个命名空间

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*父类\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
// 构造方法
Ext.wentao.Person = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

// 演示类实例方法
Ext.apply(Ext.wentao.Person.prototype, \{
job : "无",
print : function() \{
alert(String.format("姓名:\{0\},性别:\{1\},角色:\{2\}", this.name,
this.sex, this.job));
\}
\});

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*子类1\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Ext.wentao.Student = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

Ext.extend(Ext.wentao.Student, Ext.wentao.Person, \{
job : "学生"
\});

var \_student = new Ext.wentao.Student(\{
name : "张三",
sex : "女"
\});
\_student.print(); // 调用 父类方法

![None.gif][] </ script >


**7：支持类实例方法重写**

 < script type = " text/javascript " > ![None.gif][]

Ext.namespace("Ext.wentao"); // 自定义一个命名空间

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*父类\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
// 构造方法
Ext.wentao.Person = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

// 演示类实例方法
Ext.apply(Ext.wentao.Person.prototype, \{
job : "无",
print : function() \{
alert(String.format("姓名:\{0\},性别:\{1\},角色:\{2\}", this.name,
this.sex, this.job));
\}
\});

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*子类1\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Ext.wentao.Student = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

// 重写父类的 实例 方法
Ext.extend(Ext.wentao.Student, Ext.wentao.Person, \{
job : "学生",
print : function() \{
alert(String.format("\{0\}是一位\{1\}\{2\}", this.name, this.sex,
this.job));
\}
\});

var \_student = new Ext.wentao.Student(\{
name : "张三",
sex : "女"
\});
\_student.print(); // 调用 父类方法

 </ script >

**8：支持命名空间别名**

 < script type = " text/javascript " >
 

Ext.namespace("Ext.wentao"); // 自定义一个命名空间

Wt = Ext.wentao; // 命名空间的别名

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*父类\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
// 构造方法
Wt.Person = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

// 演示类实例方法
Ext.apply(Wt.Person.prototype, \{
job : "无",
print : function() \{
alert(String.format("姓名:\{0\},性别:\{1\},角色:\{2\}", this.name,
this.sex, this.job));
\}
\});

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*子类1\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Wt.Student = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

// 重写父类的 实例 方法
Ext.extend(Wt.Student, Ext.wentao.Person, \{
job : "学生",
print : function() \{
alert(String.format("\{0\}是一位\{1\}\{2\}", this.name, this.sex,
this.job));
\}
\});

var \_student = new Wt.Student(\{
name : "张q三",
sex : "女"
\});
\_student.print(); // 调用 父类方法


</ script >

**9：支持类别名**

![None.gif][] < script type = " text/javascript " > 


Ext.namespace("Ext.wentao"); // 自定义一个命名空间

Wt = Ext.wentao; // 命名空间的别名

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*父类\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
// 构造方法
Wt.Person = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

PN = Wt.Person; // 类别名

// 演示类实例方法
Ext.apply(PN.prototype, \{
job : "无",
print : function() \{
alert(String.format("姓名:\{0\},性别:\{1\},角色:\{2\}", this.name,
this.sex, this.job));
\}
\});

// \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*子类1\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
Wt.Student = function(\_cfg) \{
Ext.apply(this, \_cfg);
\};

ST = Wt.Student;

// 重写父类的 实例 方法
Ext.extend(ST, PN, \{
job : "学生",
print : function() \{
alert(String.format("\{0\}是一位\{1\}\{2\}", this.name, this.sex,
this.job));
\}
\});

var \_student = new ST(\{
name : "张q三",
sex : "女"
\});
\_student.print(); // 调用 父类方法

 </ script >


[ExtJS]: http://www.cnblogs.com/meetrice/archive/2009/07/04/1516694.html
[None.gif]: http://www.blogjava.net/Images/OutliningIndicators/None.gif