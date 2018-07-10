---
title: ExtJS 4.0 的改变--较为完整的介绍
date: 2012-01-16 10:41:53
categories: "开发"
tags:
	- Ext
	- JS

---

惯例，看之前先看看我的很久很久以前的学习笔记（就是那个Word文档，没兴趣的可以不看，不影响）：
http://wenku.baidu.com/view/ce8d3e08763231126edb1146.html ![Image 1][]

本文里面不会详细介绍某些方法函数具体如何使用，例子全部自己写的（部分参考API和ExtJS 4.0 Developer Preview），应该不会有错，提到的方法函数只提供名字，自己去API看（在此严重鄙视一些照搬API就出本书捞钱的人）

基础上的变化：

兼容性
ExtJS4最终会提供一个兼容ExtJS 3的解决方案。

沙箱模式
可是使用alias来为组件添加别名，类似以前的Ext.reg，不过alias会用不同的类别区分开来，例如，widget.xxxxx和feature.xxxxx是不一样的，虽然它们都是用alias来定义的，但是类别完全不同。

包和命名空间的改变
现在的ExtJS不再使用混乱的分包机制（其实以前的感觉更加直白），例如以前的Window，包名是Ext.Window，但是现在则是Ext.window.Window，Ext.window包下还包括了Ext.window.MessageBox等。SplitButton则是Ext.button.Split。

创建新的对象
现在ExtJS使用Ext.define函数来创建组件类，该函数还能实现自动加载JS类（uses属性，需设置Ext.Loader为开启详见下文，看不懂看API），它会自动的完成以前的ns（namespace）功能。例如下面


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.ns("Foo.bar"); 
2.  
3.  Foo.bar = Ext.extend(Ext.util.Observable,\{ 
4.  //your code here 
5.  \}); 
6.  Ext.reg("foobar",Foo.bar); 

``````````
Ext.ns("Foo.bar");

Foo.bar = Ext.extend(Ext.util.Observable,{
        //your code here
});
Ext.reg("foobar",Foo.bar);
``````````


所以现在创建一个组件应该是这样的：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define("Foo.bar",\{ 
2.   extend : "xxxxxx", 
3.   alias : "widget.foobar"
4.  //your code here 
5.  \}); 

``````````
Ext.define("Foo.bar",{
        extend : "xxxxxx",
        alias : "widget.foobar"
        //your code here
});
``````````



现在ExtJS不再使用new关键字（当然你想用也没关系），而是推荐使用Ext.create函数来解决这个问题，例如以往我们创建一个组件的代码是


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  var win = new Ext.Window(\{ 
2.  //some options 
3.  \}); 

``````````
var win = new Ext.Window({
        //some options
});
``````````


而现在则是


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  var win = Ext.create("Ext.window.Window",\{ 
2.  //some options 
3.  \}); 

``````````
var win = Ext.create("Ext.window.Window",{
        //some options
});
``````````



新的类加载方法--Class Loading
现在ExtJS可以动态加载JS文件（类）了，新的Ext.Loader类和一些其它的方法可以完成分别加载所需的JS文件，例如Ext.Loader里的setPath、require等方法函数可以做到动态加载。
如果要使用这个功能，你首先得启用它：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.Loader.setConfig(\{ 
2.   enabled: true, 
3.   paths: \{ 
4.  'My': 'my\_own\_path'
5.   \} 
6.  \}); 

``````````
Ext.Loader.setConfig({
      enabled: true,
      paths: {
          'My': 'my_own_path'
      }
});
``````````


path的意思是，当前引用这个JS的HTML文件同级的my\_own\_path目录被命名为My,所以以后该目录下的所有类名为My.xxxx的类都能被动态加载。
例如以下文件目录：
![Y6JV-MQBE-QBAZ.jpg][]
在定义的时候就是：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.Loader.setConfig(\{ 
2.   enabled: true, 
3.   paths: \{ 
4.  'NS': 'app'
5.   \} 
6.  \}); 

``````````
Ext.Loader.setConfig({
      enabled: true,
      paths: {
          'NS': 'app'
      }
});
``````````


app/person文件夹中的类名为LKPerson的定义代码为：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define("NS.person.LKPerson", \{ 
2.   extend : "Ext.panel.Panel", 
3.   alias : "widget.lkperson",//当然，这个属性不是必须的 
4.   border : false, 
5.   initComponent : function()\{ 
6.  this.callParent(arguments);//这个arguments你懂，不懂Google 
7.   \} 
8.  \}) 

``````````
Ext.define("NS.person.LKPerson", {
	extend : "Ext.panel.Panel",
        alias : "widget.lkperson",//当然，这个属性不是必须的
	border : false,
        initComponent : function(){
                this.callParent(arguments);//这个arguments你懂，不懂Google
        }
})
``````````


注意一点的就是，NS.person.LKPerson中的LKPerson就是文件名称（换句话说文件名必须是LKPerson且必须在person目录下）

好了，下面看看动态加载的两种方式：
require的用法如下：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.require("Foo.bar"); 
2.  
3.  Ext.define("AA.bb.CC",\{ 
4.  //some options 
5.  \}); 

``````````
Ext.require("Foo.bar");

Ext.define("AA.bb.CC",{
        //some options
});
``````````



require的意思是：在这个类（AA.bb.CC）被加载之前必须要加载Foo.bar并且被实例化（虽然好用但是劝各位不要滥用）。

uses的用法如下：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define("AA.bb.CC",\{ 
2.   uses:\["Foo.bar"\] 
3.  \}); 

``````````
Ext.define("AA.bb.CC",{
        uses:["Foo.bar"]
});
``````````



uses的意思是：在这个类（AA.bb.CC）在运行过程中要用到Foo.bar这个类，用到的时候再加载。

其它的就不多解释，具体看API（这句话我最后说一遍 ![Image 1][] ）

混入类
将一个类混入到另外一个类中，创建的时候一同创建：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define("Dog",\{ 
2.   sayHello : function()\{ 
3.   alert("AAAA") 
4.   \} 
5.  \}) 
6.  
7.  Ext.define("Animal",\{ 
8.   mixins:\{ 
9.   dog:"Dog"
10.  \} 
11. \}); 
12. 
13. 
14. Ext.onReady(function()\{ 
15. var an = Ext.create("Animal"); 
16.  an.mixins.dog.sayHello(); 
17. \}) 

``````````
Ext.define("Dog",{
	sayHello : function(){
		alert("AAAA")
	}
})

Ext.define("Animal",{
	mixins:{
		dog:"Dog"
	}
});


Ext.onReady(function(){
	var an = Ext.create("Animal");
	an.mixins.dog.sayHello();
})
``````````



静态方法
现在所有的类（是所有的ExtJS类，Ext.define定义的）都提供静态方法，可以很方便的定义：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define('Computer', \{ 
2.   statics: \{ 
3.   factory: function(brand) \{ 
4.  // 'this' in static methods refer to the class itself 
5.  returnnewthis(brand); 
6.   \} 
7.   \}, 
8.  
9.   constructor: function() \{ ... \} 
10. \}); 
11. 
12. var dellComputer = Computer.factory('Dell'); 

``````````
Ext.define('Computer', {
     statics: {
         factory: function(brand) {
             // 'this' in static methods refer to the class itself
             return new this(brand);
         }
     },

     constructor: function() { ... }
});

var dellComputer = Computer.factory('Dell');
``````````



Config
这个东西很时髦，看看用法：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define("Foo",\{ 
2.   config: \{ 
3.   title: 'Default Title'
4.   \}, 
5.   constructor: function(cfg) \{ 
6.  this.initConfig(cfg); 
7.   \} 
8.  \}); 
9.  
10. 
11. Ext.onReady(function()\{ 
12. var an = Ext.create("Foo",\{ 
13.  title:"My Title"
14.  \}); 
15.  console.log(an.getTitle()); 
16. \}); 

``````````
Ext.define("Foo",{
    config: {
        title: 'Default Title'
    },
    constructor: function(cfg) {
    	this.initConfig(cfg);
    }
});


Ext.onReady(function(){
	var an = Ext.create("Foo",{
		title:"My Title"
	});
	console.log(an.getTitle());
});
``````````


运行以下看看会出现什么？是My Title，config会自动生成getter和setter还有apply方法。

Ext.env.Browser
这个东西很好很强大。。。。记得以前有Ext.isIE、Ext.isFF等方法判断浏览器，这次ExtJS 4不仅保留了以前的函数，还提供了更为强大Ext.env包来干这些事情，这个包下面还有其它两个类：
Ext.env.OS，顾名思义判断操作系统的，印象中ExtJS3中好像也有，不过这次新增了一些操作系统（主要是移动领域的操作系统）。


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  if (Ext.os.is.Windows) \{ 
2.  // Windows specific code here  
3.  \} 
4.  
5.  if (Ext.os.is.iOS) \{ 
6.  // iPad, iPod, iPhone, etc. 
7.  \} 

``````````
if (Ext.os.is.Windows) {
     // Windows specific code here
}

if (Ext.os.is.iOS) {
     // iPad, iPod, iPhone, etc.
}
``````````



Ext.env.FeatureDetector,这个绝对是新加的，主要用于判断HTML5和CSS3的，例如


 *  CSS3/动画/转换
 *  Canvas/ SVG/ VML
 *  触摸屏是否可用/方向
 *  地理位置（HTML5的东西相信不陌生吧？）
 *  SqlDatabase
 *  WebSockets
 *  History
 *  音频
 *  视频


Lang包的修改
不知道lang是什么意思？（想想java.lang.String和java.lang.Number吧）
现在ExtJS 4一直在为新的ECMAScript标准做准备，这就是为什么要有这个包存在的原因，新的ECMAScript标准大家可以在ActionScript3中详细看看，AS3基本上是遵循了下一代ECMAScript标准的，例如数据类型String、Number、Array、uint等等，ExtJS 4为了能与下一代的ECMAScript标准兼容，提前提供了这些东西的JS实现。

Ext.Function
这个东西不知道大家平时用不用（也许大家用了没察觉到），我是经常用的。例如在以往的ExtJS版本中的代码：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  function foo()\{ 
2.  //这里 
3.  \}.createDelegate(scope,arguments); 

``````````
function foo(){
       //这里
}.createDelegate(scope,arguments);
``````````


现在变成了（这里使用了一个综合的例子，大家可以重点看看bind的用法）：


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define("Foo",\{ 
2.   config:\{ 
3.   bar:"I don't need sex,the government fucks me every day!"
4.   \}, 
5.   constructor : function(cfg)\{ 
6.  this.initConfig(cfg); 
7.   \} 
8.  \}); 
9.  
10. function fun(str)\{ 
11. if(str)\{ 
12.  alert(str); 
13. return; 
14.  \} 
15.  alert(this.getBar()); 
16. \} 
17. 
18. Ext.onReady(function()\{ 
19. var foo = Ext.create("Foo"); 
20.  Ext.bind(fun,foo)();//后面的括号你懂 
21. //如果要传递参数可以以数组的形式放入，例如这样： 
22. //Ext.bind(fun,foo,\["私はセックスを必要としない、政府は毎日私をファック！"\])(); 
23. \}); 

``````````
Ext.define("Foo",{
	config:{
		bar:"I don't need sex,the government fucks me every day!"
	},
	constructor : function(cfg){
		this.initConfig(cfg);
	}
});

function fun(str){
	if(str){
		alert(str);
		return;
	}
	alert(this.getBar());
}

Ext.onReady(function(){
	var foo = Ext.create("Foo");
	Ext.bind(fun,foo)();//后面的括号你懂
	//如果要传递参数可以以数组的形式放入，例如这样：
	//Ext.bind(fun,foo,["私はセックスを必要としない、政府は毎日私をファック！"])();
});
``````````


好了，我承认我有点那啥，不过希望不影响你继续读下去的兴趣。

相同的还有
ExtJS 3中的


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  myFunction.defer(1000, scope); 

``````````
myFunction.defer(1000, scope);
``````````



变成了4中的


Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.defer(myFunction, 1000, scope); 

``````````
Ext.defer(myFunction, 1000, scope);
``````````


还有callBack变成了pass，其它的自己去发掘。

ExtJS的工具，
就是它：
![F2IA-FBN3-MI6F.jpg][]
使用工具来编译你的JS，别想歪了，我知道JS是解释性的，详细的先看看这篇文章关于JSBuilder的作用：
http://hi.baidu.com/mallor/blog/item/5aec22f3e3dcbadc0b46e074.html

ExtJS的senchaSDKtools里面还提供了theme的制作（命令行的？？！！！），如果嫌ExtJS审美疲劳了可以试试这个。没玩过，所以不多解释，这里就告诉大家有这么回事儿就行了。

关于大家最关心的数据
这个Store想必是最关心的吧？（好了，外面貌似下冰雹了我晕，楼顶啪啪的！没时间去核实有北京的2011年七月二十六号晚上21：40：00左右的网友如果核实了留言证实一下）。
嗯，说到冰雹，我觉得转载这篇文章的人应该注意一下告诉出处，这篇文章来自流水无心的ITEYE博客http://andy-ghg.iteye.com/

扯远了，再回来继续说，这个Store现在改变可不小，详细的参看一下这个文章
\[url src='http://www.sencha.com/blog/ext-js-4-anatomy-of-a-model/'\]Countdown to Ext JS 4: Anatomy of a Model\[/url\]
好了，我知道我很懒，但是我可以简单说一下：modal代替了以往store中fields:\[""\]的功能，但是我没用（实际使用有问题－ －！学艺不精），其实它是一个对象。。。对的是对象，MVC中的Model，好了，具体看API中关于MVC的介绍吧。
Store中有一个变化就是baseParams变程了extraParams，注意下。每一个Store必须（？是否必须我也不知道，不过我现在是这么用的）要有一个Proxy且必须给一个type，现在的Store是这样写的：



Js代码 ![复制代码][Image 1] ![收藏代码][Image 1]![Image 1][]

1.  Ext.define('User', \{ 
2.   extend: 'Ext.data.Model', 
3.   fields: \[ 
4.   \{name: 'name', type: 'string'\}, 
5.   \{name: 'age', type: 'int'\} 
6.   \] 
7.  \}); 
8.  
9.  Ext.create("Ext.data.Store",\{ 
10.  modal:"User", 
11.  proxy\{ 
12.  url : "xxxx.do", 
13.  type : "ajax"
14.  \} 
15. \}) 

``````````
Ext.define('User', {
    extend: 'Ext.data.Model',
    fields: [
        {name: 'name',  type: 'string'},
        {name: 'age',   type: 'int'}
    ]
});

Ext.create("Ext.data.Store",{
	modal:"User",
	proxy{
		url : "xxxx.do",
		type : "ajax"
	}
})
``````````


确实像那么回事儿，但是我不买账。。。在实际项目中我还是用我的fileds，在没彻底搞明白之前，不太敢这么弄![Image 1][] 。

当然Store的改动远远不止这些，例如Proxy中新增了一个localStorage（Ext.data.proxy.LocalStorage）用于过渡到HTML5的localStorage等等。

Draw绘图
这个东西喜欢吗？我喜欢嘿嘿
ExtJS4中提供了绘图，夸浏览器的，它内部实现了Canvas、SVG、VML等绘图方法，所以不同的浏览器它会自动使用该浏览器支持的绘图方式。支持IE6789、基于Gecko的浏览器(FF)、基于WebKit内核的浏览器(Chrome)。

FX
从ExtJS提供这个以来我就不怎么用，原因很简单，浪费我的资源。。。。在新的ExtJS中，提供了类似Flex中关于动画的包方法Ext.fx.target.\*，因为我不怎么用，所以不再阐述。


布局
这里的改变也很大，但是我觉得我在这个博客里说再多也没有你自己去运行它的例子来得要直观。


好了，说了这么多，当然没有说完。。。。更多细节的改变的大家自己去发掘，推荐一篇文章《ExtJS 4.0 Developer Preview》，地址我忘了，网络时代相信对你来说找到它不难。


最后做一下小小的广告：
我自己写的AS3（Flex）流程图例子快要完工了～！以前提供了一个简易的Demo版本，这次是经过重大修改后的（不是说以前有重大的BUG，而是以前的实在太难用－ －！），虽然是例子，但是如果您喜欢，完全可以用来做二次开发～！嘿嘿。下面是最新版的截图：
![点击查看原始大小图片][N3EN-MBIM-JQMZ.jpg]

目前提供了这些小组件：
![BM2Q-ENJY-7RYM.jpg][]

最后的最后
从ExtJS 2.0一直用到4.0，从Flex 3用到Flex 4.5，感慨很多，在ExtJS成长的过程中我已渐渐的从前端转向了后端，不是因为我不再热爱前端和RIA，而是因为我觉得多方的了解与涉及能让自己得到更好的升华，目前专注j2ee开发，主要涉及Spring、Spring Security、CAS SSO、JMS ActiveMQ、Hibernate、jBPM、Oracle。
其它的包括C\#、C++QT4、jQuery等等也装模作样的学了些，所以不要再讨论某个语言某个框架的优劣，有这个时间可以学到很多很多东西。何必眼光这么狭隘呢？![Image 1][]，争论太浪费精力。少点争论多点实际付出。![Image 1][]

Happy coding～！
\---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

以上全文转载自:http://andy-ghg.iteye.com/blog/1133456

小韩感谢此网友的贡献，同时欢迎加我Q和群，希望共同学习、交流、共进。


[Image 1]: 
[Y6JV-MQBE-QBAZ.jpg]: /pro/os/crawler/Y6JV-MQBE-QBAZ.jpg
[F2IA-FBN3-MI6F.jpg]: /pro/os/crawler/F2IA-FBN3-MI6F.jpg
[N3EN-MBIM-JQMZ.jpg]: /pro/os/crawler/N3EN-MBIM-JQMZ.jpg
[BM2Q-ENJY-7RYM.jpg]: /pro/os/crawler/BM2Q-ENJY-7RYM.jpg