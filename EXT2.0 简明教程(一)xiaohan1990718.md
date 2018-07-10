---
title: EXT2.0 简明教程(一)
date: 2012-02-09 19:19:57
categories: "开发"
tags:
	- EXT

---

Ext全部转自：DOJO中国：EXT2.0 简明教程，网址：http://www.dojochina.com/?q=node/562

第 1 章 闪烁吧！看看extjs那些美丽的例子。

## 1.1. 一切从extjs发布包开始 ##

非常幸运的是，我们可以免费去extjs.com下载ext发布包，里边源代码，api文档，例子一应俱全。不过要是想访问svn获得最新的代码，就要花钱了。不过我们现阶段只要这个免费的发布包就可以了，通过里边的范例，可以让我们体验一下ext的风范。

下载地址：http://www.extjs.com/download。到写文档的此时此刻，咱们可以选择ext-1.1.1或者是ext-2.0下载。明显可以看出来ext-2.0的版本高，12月4日终于正式发布了，尚未经过详细测试，所以不敢说什么。下面我们把两个版本都介绍一点儿。

## 1.2. 看看ext-1.1.1的文档 ##

docs目录下是api文档，基本上所有的函数，配置，事件都可以在里边找到，直接打开index.html就可以查看，左侧菜单还包含了对examples目录下例子的引用，不过有些例子需要使用json与后台做数据交换，必须放到服务器上才能看到效果。还有一些后台代码是使用php编写的，如果想看这些例子的效果，还需要配置php运行环境。

如果你用java，而且jdk在1.5以上，不如直接装个resin-3方便，它可以跑php呢。

## 1.3. 看看ext-2.0的文档 ##

api文档依然在docs目录下，虽然可以看到左边的菜单，但是点击之后，右侧的api页面都是靠ajax获得的，所以不能直接在本地查看了，你必须把整个解压缩后的目录放到服务器上，通过浏览器访问服务器，才能看到。

问为什么docs打不开，只能看到一直loading的兄弟，都是因为没把这些东西放到服务器上的原因。

2.0中的api文档中没有例子的链接了，你需要自己去examples目录下找你需要的例子，然后打开看，当然还是有一些例子需要放在服务器上才能看到效果。

## 1.4. 为什么有的例子必须放在服务器上才能看到效果？ ##

因为有些例子里，用到Ajax去后台读取数据，如果没在服务器上，Ajax返回的状态一直是失败，也无法获得任何数据，所有就看不到正确的效果。不过以前在extjs.com论坛上看到过有人写了localXHR，可以让ajax从本地文件系统获得数据，这样也许就可以摆脱服务器的束缚了。

## 1.5. 为什么自己按照例子写的代码，显示出来总找不到图片 ##

ext里经常用一个空白图片做占位符号，然后用css里配置的背景图片做显示，这样有利于更换主题。这个空白图片的名字就是Ext.BLANK\_IMAGE\_URL，默认情况下它的值是BLANK\_IMAGE\_URL : "http:/"+"/extjs.com/s.gif"。虽然图片很小，也要去网上下载，一旦下载失败就显示找不到图片了。

看到这里可能有人奇怪了，为什么examples下的例子没有找不到图片的问题呢？看来你没有仔细看例子那些代码呢，每个例子都引用了../examples.js。在examples.js里设置了Ext.BLANK\_IMAGE\_URL = '../../resources/images/default/s.gif';。所以要解决自己写的例子找不到图片的问题，只需要照examples.js里修改s.gif的本地路径就可以了。很简单吧？

## 1.6. 我们还需要什么？ ##

1.  介于本人对firefox的喜爱，以及firebug在调试js过程中的便利，隆重向您推荐firefox+firebug的开发组合。再说了ext开发者也都是倾向于firefox开发的，所以一般都是在firefox上跑的好好的，放到ie上就出问题。这也跟ie自身的问题有些关系，可是目前ie占据90%的浏览器市场，最后我们还是需要让自己的项目在ie上跑起来，所以要求我们能写出跨浏览器的js来。
    
    firebug的好处在于，可以显示动态生成的dom，你甚至可以在firebug里直接对dom进行修改，而这些修改会直接反应到显示上。太厉害了
    
    firebug提供的console，可以直接执行js脚本，配置console.debug，console.info，console.error等日志方法更便于跟踪。
    
    对于ajax发送接收的数据，firebug都可以显示出来，并且可以查看发送的参数，以及返回的状态和信息。

## 1.7. 入门之前，都看helloworld。 ##

为了照顾连最基本应用都跑不起来的同志，我们给出两个入门版helloworld范例，并结合讲解，领你入门呢。

### 1.7.1. 直接使用下载的发布包 ###

1.  先去http://www.extjs.com/download下载zip格式的发布包
2.  随便解压缩到什么目录下，不管目录名是什么，最后应该看到里边是这样的目录结构。
    
    ![01-01.png][]
3.  现在我们利用它的目录结构，写一个helloworld例子。
    
    进入上图中的examples目录，新建一个helloworld目录，helloworld目录下新建一个helloworld.html文件，将下列内容复制进index.html文件中。
    
    ``````````
    <link rel="stylesheet" type="text/css" href="../../../resources/css/ext-all.css" />
    <script type="text/javascript" src="../../adapter/ext/ext-base.js"></script>
    <script type="text/javascript" src="../../ext-all.js"></script>
    <script type="text/javascript" src="../examples.js"></script>
    
    <script>
    Ext.onReady(function(){
        Ext.MessageBox.alert('helloworld', 'Hello World.');
    });
    </script>
    ``````````
4.  双击helloworld.html打开页面，效果如下：
    
    ![01-02.png][]

很高兴的告诉您，咱们的helloworld范例已经正确的执行了，下一步你最好把examples目录下的例子都看一看，再看看里边的代码怎么写的，好好感受一下ext的风范，再继续下去。

### 1.7.2. 只把必要的东西放进项目中 ###

想把ext放入自己的项目，需要自己整理一下，因为发布包里的东西并非都是必要的，比如文档，比如例子，比如源代码。

必要的最小集合是这样：ext-all.js，adapter/ext/ext-base.js，build/locale/ext-lang-zh\_CN.js和整个resources目录。

1.  ext-all.js，adapter/ext/ext-base.js就包含了ext的所有功能，所有的js脚本都在这里了。
2.  build/locale/ext-lang-zh\_CN.js是中文翻译。
3.  resources目录下是css样式表和图片。

自己的项目里只需要包含这些东西，就可以使用ext了。使用时，在页面中导入：

``````````
<link rel="stylesheet" type="text/css" href="${放置ext的目录}/resources/css/ext-all.css" />
<script type="text/javascript" src="${放置ext的目录}/ext-base.js"></script>
<script type="text/javascript" src="${放置ext的目录}/ext-all.js"></script>
<script type="text/javascript" src="${放置ext的目录}/ext-lang-zh_CN.js"></script>
``````````

请注意js脚本的导入顺序，其他的就可以自由发挥了。


[01-01.png]: http://203.93.254.59:8889/extdoc/shared/images/01-01.png
[01-02.png]: http://203.93.254.59:8889/extdoc/shared/images/01-02.png