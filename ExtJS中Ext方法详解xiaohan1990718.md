---
title: ExtJS中Ext方法详解
date: 2012-09-25 11:49:59
categories: "开发"
tags:
	- Ext
	- JS

---

1、**Ext.apply**(Object obj, Object config, Object defaults ) : Object
将config中的所有属性复制到obj中，如果配置了defaults，则先将defaults中的属性传入obj，然后再将config中属性传入，一般defaults用于定义一些默认值。
注意：每个参数都必须是对象object，而不能是function或其他。
创建object可以通过new function()\{\}、new Object()、\{\}等方法创建。

2、**Ext.emptyFn**: Function
用于返回一个空函数，便于在程序中创建空函数。Ext.emptyFn返回function()\{\}

3、**Ext.applyIf**(Object obj, Object config) : Object
功能如同Ext.apply,但是只把config中存在而obj不存在的属性复制过去。

4、**Ext.addBehaviors**( Object obj ) : void
为页面中一个或多个元素添加事件
元素使用css规则查找，其中元素与事件用@隔开


Ext.addBehaviors(\{
//为id为foo的元素下的所有a元素添加click事件
'\#foo a@click' : function(e, t)\{
// do something
\},
// 为多个选择器添加相同的事件(mouseover)。在@之前使用逗号分开
'\#foo a, \#bar span.some-class@mouseover' : function()\{
// do something
\}
\});



5、**Ext.id**( \[Mixed el\], \[String prefix\] ) : String
返回一个唯一的id值。
如果只需要获取一个唯一的id值，则直接调用Ext.id()；
如果需要为某个元素设定一个唯一的id值并返回id则调用Ext.id(el)，el为元素Id、Dom对象或Ext的Element对象。
如果需要指定特定的前缀，则需要传入第二个参数，如Ext.id(el,”myPrix-”)，默认前缀为ext-gen，如默认返回id可能为ext-gen4，指定了前缀后可能返回myPrix-4。

6、**Ext.extend**( Object subclass, Object superclass, \[Object overrides\] ) : void
实现对象继承，目前还不太了解具体原理 ？？？

7、**Ext.namespace**( String namespace1, String namespace2, String etc ) : void
创建命名空间：
如Ext.namespace("Company","MyNS.mydata","Data.format.string")
然后可以创建如MyNS.mydata.doit=function()\{…\}的接口
注：命名空间的简易调用：Ext.ns()，在Ext Api中未给出此用法。

8、**Ext.urlEncode**( Object o ) : String
将一个json对象转换称url参数串，支持通过数组为一个参数设定多个值。
如将\{a:1,b:2,c:\[1,3,5,7\]\}转换为a=1&b=2&c=1&c=3&c=5&c=7

9、**Ext.urlDecode**( String string, \[Boolean overwrite\] ) : Object
将url参数串转换为json对象，overwrite如果为true，则后面的同名参数值覆盖前面的同名参数值（默认为false即不覆盖而以数组形式返回）。
如



Ext.urlDecode("a=1&b=2&c=1&c=3&c=5&c=7")
返回的对象内容为\{a:1,b:2,c:\[1,3,5,7\]\}
Ext.urlDecode("a=1&b=2&c=1&c=3&c=5&c=7",true)
返回\{a:1,b:2,c:7\}



10、**Ext.each**( Array/NodeList/Mixed array, Function fn, Object scope ) : void
遍历array并对每项分别调用fn函数。如果array不是数组则只执行一次。
如果某项fn执行结果返回false（必须是false，undefined无效），遍历退出，后面的array项将不被遍历。
遍历过程中每次为fn传入参数分别为\[当前数组项\]，\[当前索引\]和\[数组array\]三个参数。
Scope用于设定fn函数中的this指针。
如



Ext.each(\[1,3,5,7\],function(v,i,a)\{
alert("index: "+i+" value: "+v+" array.length："+a.length)
\});


将循环弹出：
index:0 value:1 array.length：4
index:1 value:3 array.length：4
index:2 value:5 array.length：4
index:3 value:7 array.length：4




Ext.each(\[1,3,5,7\],function(v,i,a)\{
alert("index: "+i+" value: "+v+" array.length："+a.length);
return v!=5; //到第三项后遍历退出
\});


将循环弹出：
index:0 value:1 array.length：4
index:1 value:3 array.length：4
index:2 value:5 array.length：4

11、**Ext.combine**(arg1,arg2..argn) : Array //该方法在Ext2不推荐再使用
用于实现对数组的合并，如果是字符串则作为只有一项的数组合并。
如



var a1=\[1,3,5\],b1=\["a","b","c"\];var c1="xxyznbde";
Ext.combine(a1,b1,c1) 返回\[1,3,5,a,b,c,xxyznbde\]



12、**Ext. escapeRe**( String str ) : String
将属于正则里的特殊字符进行转义。
如


Ext.escapeRe("(ab)$\\sa342\{\}\[dd\]")将返回\\(ab\\)\\$sa342\\\{\\\}\\\[dd\\\]。



13、**Ext.callback**(cb, scope, args, delay) :void //该方法为Ext的内部方法
调用一个函数或延迟调用一个函数。
Cb:调用的函数。
scope:cb中this指针。
args：传如cb的参数，以数组形式表示。
delay：延迟多少毫秒执行cb。
如


Ext.callback(function(x,y)\{alert(x+y)\},this,\[3,5\],1000);将于1秒钟后弹出8，即3+5的结果。



14、**Ext.getDom**( Mixed el ) : HTMLElement
根据传入的id/dom节点/Ext的Elemenet对象，返回其dom对象。
如alert(Ext.getDom("a").innerHTML);或
alert(Ext.getDom(document.getElementByIdx("a")).innerHTML);
将返回id为a的元素的innerHTML内容。

15、**Ext.getDoc()**//Ext.getBody() : Ext.Element
分别返回页面的document对象和body对象，返回值为Ext的Element对象，而非Dom对象。

16、Ext.getCmp( String id ) : Ext.Component
根据传入的html元素id返回该元素的组件类型，返回值为Ext的Component对象。
必须保证该id对象的元素是Ext的一个内部组件（通过Ext创建的组件），否则什么都不返回。

17、**Ext.num**( Mixed value, Number defaultValue ) : Number
验证value是否是一个数字，如果是则直接返回否则返回defaultValue。
如


alert(Ext.num(5,7))返回5，alert(Ext.num("5",7)) 返回7



18、**Ext.destroy**( Mixed arg1, Mixed (optional), Mixed (optional) ) : void
销毁创建的Element或组件(Component)，即销毁其所有的事件监听，dom节点，并调用对象本身的destory方法（如果存在的话），传入的参数类型为Ext.Element或Ext. Component，可以一次性传入多个对象进行销毁。
如


Ext.destory(menu,el,Button);会销毁menu,el,Button三个对象。



19、**Ext.removeNode**(htmlElement el): void //Ext内部方法
删除指定的dom节点。传入参数为dom对象。
如


Ext.removeNode(document.getElementByIdx("ab"));



20、**Ext.type**( Mixed object ) : String
返回传入的对象的类型。
包括如下类型：
string,number,boolean,function,object,array,regexp,element,nodelist,textnode,whitespace
如



Ext.type("ab")返回string
Ext.type(20)返回number
Ext.type(\[3,5,6\])返回array
Ext.type(/reg/)返回regexp
Ext.type(document.body)返回element。



21、**Ext.isEmpty**( Mixed value, \[Boolean allowBlank\] ) : Boolean
检查一个值是否为null/undefined或是否是空，如果是则返回true。
如果传入allowBlank为true，则只检查是否为null或undefined。
如：



Ext.isEmpty("a")返回false，
Ext.isEmpty("")返回true，
Ext.isEmpty("",true)返回false，
Ext.isEmpty(null)返回true。
