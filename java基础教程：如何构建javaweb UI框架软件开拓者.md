---
title: java基础教程：如何构建javaweb UI框架
date: 2018-01-30 10:49:22
categories: "开发"
tags:
	- Java
	- 程序员
	- 技术
	- 鼠标
	- 编程语言

---

![java基础教程：如何构建javaweb UI框架][java_javaweb UI]

这几天分享的内容主要是基础的东西，今天咱们就说说UI框架。

一、说UI能延展出一丢丢的东西来，光java就有swing，swt/jface乃至javafx等等UI toolkit，在桌面上它们甚至都不是主流，在web端又有canvas、svg等等。

常用的UI框架有哪些?

Bootstrap、jQuery UI、Pure CSS、Semantic UI、Ace Admin、BootMetro

基于这些UI工具包框架，又产生了大量通用的或者业务性的UI框架，比如Draw2d、GEF、easyUI乃至国内的EChart、白鹭等等。

这些框架的业务范围各异，一个程序员的时间和精力有限，你不可能全部都掌握，又不能预言出是哪一个将来会独步天下，甚至，连当前哪一个最流行，都够打一阵嘴炮。

那我们应该学什么？如果我们只有一个GC（graphic creatorcapability）的时候，如何设计一套UI框架出来，了解了这些，在学习新的UI框架的时候，会更加容易。同时，这里提到的分析思路也可以应用到其他类型框架中，辅助学习。

切记，**1、复杂由简单构成；2、构成具备规律**。

可想而知，再复杂的框架，划拉到底层的时候，也都是你早就应当掌握的基础知识。不同的框架需要��基础知识可能不一样，但是规律（思想）大多是接近的。

那么什么是GC？每一个使用过上面提及的UI toolkit的程序员，应该都注意到了，它们很多都有提供“绘制”功能。在SWT中，提供该能力的是org.eclipse.swt.graphics.GC，在H5 canvas中，则是CanvasRender（也就是canvas element\#getContext(‘2d’)得到的对象）。

它们有什么共同点？

可以设置ARGB信息，可以调用来绘制图片、线段、形状，等等。

为了方便起见，这里统一称为GC，它是UI框架的基础。

来思考一个问题，一个下图所示的界面，如果用GC自行绘制的话，你要怎么做？

![java基础教程：如何构建javaweb UI框架][java_javaweb UI 1]

不用想太多，完整实现是不科学的，写到颓，颓到秃。来看下最常见的解决方案吧：

把该界面上的各个部件拆开来，相同特性的归为一类，每一类都提供一个绘制的方法，比如一个按���，就需要GC绘制四条边、阴影线以及其中的文本。

然后，让遍历所有的部件，让文本的归文本，按钮的归按钮。

有没有突然感觉简单了？这就是所谓的UI框架干的事儿。

**二、如何构建这个UI框架呢？**

本文采取的思路是：**面向对象**建模。

具备面向对象知识的你，应当能分析出以下结论：控件，控件的布局，以及各种事件的处理，这三个元素组成了一个基本的UI框架。

1、**控件**

按钮、文本框等可操作对象以及包裹它们的容器，这里称之为控件。

所以，我们需要创建出以下对象，继承关系以树形结构表示：

![java基础教程：如何构建javaweb UI框架][java_javaweb UI 2]

其中父子级只表达继承关系，Control和Composite它们的实体关系图，则可能是如下所示：

![java基础教程：如何构建javaweb UI框架][java_javaweb UI 3]

其中，Control提供两个方法：

paint(GC)负���调用GC，来绘制自身。

getBounds()负责提供当前控件的位置、大小信息，一般包括x,y,width,height。

Composite作为复合控件，它既具备Control的两个方法，用于正确的绘制自身，又具备一个children列表，里面全都装的是Control，当它的paint方法调用的时候，应当迭代自己的所有children，调用它们的paint。

具备此种结构之后，任何UI界面是不是都成为了某种单根结构？

根节点是一个Root Composite，叶子节点则是具体的某个Control。再联想之前的打印窗口，它的实体结构大概会是这样：

![java基础教程：如何构建javaweb UI框架][java_javaweb UI 4]

来活动活动思维吧，这是什么数据结构？各个控件的paint()方法调用顺序是怎样的？

2、**布局**

明明是有bounds的，为什么还需要“布局”呢？

其实啊，bounds（x,y,width,height）也可以视为一种布局，我称之为自由布局，这种布局其��并不好，结论太粗暴难以接受么？大概讲解一下：

**你需要非常精确的控制x,y,width,height四个变量**，设想你要制作一张两行四列的表格，每个单元格你都得控制它们的位置，bounds信息如下所示：

\[0,0,100,20\],\[100,0,100,20\],\[200,0,100,20\],\[300,0,100,20\],

\[0,20,100,20\],\[100,40,100,20\],\[200,60,100,20\],\[300,80,100,20\]

如此你可以推论出一个公式，设i为行号，j为列号，单个cell的bounds信息公式为\[i\*width,j\*height,width,height\]，注意到问题没有，你需要自己维护一个嵌套循环，来为每一个单元格赋值。

**控制相对位置需要花费大力气**。明确一个事实，在该“自由布局”里，child的范围是有可能超出parent的边框的（因为bounds的x,y目前指代的是GC使用到的x,y，也就是整个画布的基准点），除非，你把每一个child的计算公式都改为\[i\*width+offsetX,j\*height+offsetY,width,height\]，这里的offset代表parent的绝对位置。

**很难控制缩放**。比如你要对上述的表格进行缩放，则你需要修改bounds信息，确定缩放的策略，比如整体缩小一个zoom值，列出的公式大概会变成这个样子\[i\*width\*zoom,j\*height\*zoom,width\*zoom,height\*zoom\]

仅仅说明一个方式不好，并不能证明其他的方式好，很多人已经想到了，我们可以把上面的这些“公式”抽离出来，整理出可复用的代码。如果有其他的布局方案，也可以整理出对应的复用代码，在paint之前应用上去，不就好了么？

对，其实，这个可复用的方案策略，也就是“布局”。

以伪码说明实现方式：

define Composite\{

LayoutManager layoutManager;

/\*\*

\*绘制

\*/

void paint()\{

layout();

//dopaint

\}

/\*\*

\*执行布局

\*/

void layout()\{

layoutManager.layout(this);

\}

\}

define LayoutManager\{

void layout(Composite parent)\{

int index=0;

//遍历所有的children，获取它们的layoutData，修改它们的位置

foreach(Control child:parent.getChildren())\{

//根据child的index以及布局配置计算出偏移量

offset=computeOffset(index);

//获取child的布局数据

LayoutData data=child.getLayoutData();

//使用布局数据修改child的bounds信息

data.computeBounds(child,index,offset);

index++;

\}

\}

\}

从上面伪码我们可以看出，控件具备layoutData这个成员函数，容器（复合控件）具备layout这个���员函数.

**layout**用于规定该容器内部的布局类型（比如网格类型GridLayout），整体的布局规划（体现在上述代码中的offset对象，为下一个需要布局的child提供偏移量）。

**layoutData**负责具体的某一个child在整体中的排布。

由于封装在layout和layoutData中的都是算法（体现在computeXXX方法中），所以，我们可以灵活的规定、服用不同的组合方式，比如把一个控件布置在容器的整体居���位置。

完成了这些，你的UI框架就能用于展示各种视图了。

3、**事件分发**

仅仅用于展示自然是不够的，不然UI框架完全可以称之为视图框架，这个UI框架应当可以接收各种类型的键盘、鼠标输入。

以H5的canvas为例，我们知道canvas是可以添加鼠标键盘监听的，你完成了控件和布局，点击按钮控件，响应事件的是按钮还是canvas？

自然是canvas，浏览器哪里知道你写了个“按钮���出来。

对于不熟悉MVC模式的同学可能会有些疑惑，一个“绘制”上去的假按钮，要如何响应事件呢？

我们再来看下这棵树：

![java基础教程：如何构建javaweb UI框架][java_javaweb UI 5]

推论如下：

1、根容器的bounds等同于canvas的位置大小。

2、如果canvas接收到了鼠标事件，鼠标一定位于某个根容器下某个控件位置上。

3、树遍历控件，即可快速找到事件发生的时候，鼠标处于哪���控件之上。

鉴于JS在某些浏览器上的执行效率（我不是说微信），我们还可以做更多的优化。这里抛砖引玉：

1、引入**层**（layer）的概念，对树结构再做一个横向划分，优先查找层数高的控件，也可以让某些层的控件不参与查找。

2、对控件本身设置**可点击属性**，毕竟if判断的速度要比计算(x,y)是否位于bounds范围内要快。

**如果以上分享对你有所帮助，可以多多转发和��注，初学者有什么不懂的可以私信我，需要系统学习资料和系统学习框架图的同学，可关注小编头条号，欢迎留言评论和私信小编。【私信方法】文章上方处点击“作者头像”，进入作者首页，在作者主页上方点击“关注”旁边的“发私信”即可。私信内容：学习资料。**


[java_javaweb UI]: /pro/os/crawler/FZMN-JMEB-M3UU.jpg
[java_javaweb UI 1]: /pro/os/crawler/VNZY-QMBQ-UEFR.jpg
[java_javaweb UI 2]: /pro/os/crawler/U2EQ-QMIV-3ANR.jpg
[java_javaweb UI 3]: /pro/os/crawler/ZJAY-R3AE-3EFY.jpg
[java_javaweb UI 4]: /pro/os/crawler/2U7N-ZIJ7-BEFN.jpg
[java_javaweb UI 5]: /pro/os/crawler/QAYM-RFVI-V3ME.jpg
 *  **原文作者：** 软件开拓者
 *  **原文链接：** https://www.toutiao.com/item/6516670393544081924/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。