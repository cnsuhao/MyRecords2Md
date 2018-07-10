---
title: Java 9揭秘-创建你的第一个模块
date: 2018-04-22 16:01:50
categories: "开发"
tags:
	- Java
	- NetBeans
	- 编程语言
	- Windows
	- IDE

---

![Java 9揭秘-创建你的第一个模块][Java 9_-]

在这个章节中，主要介绍以下内容：

 *  如何编写模块化的Java程序。
 *  如何编译模块化程序。
 *  如何将模块化代码打包成模块化的JAR文件。
 *  如何运行模块化程序。

在本章中，将介绍如何使用模块 —— 从���写源代码到编译，打包和运行程序。 本章分为两部分。 第一部分显示使用命令行编写和运行模块程序的所有步骤。 第二部分使用NetBeans IDE重复相同的步骤。

到目前，NetBeans IDE仍在开发中，并且不支持所有JDK 9功能。 例如，目前需要NetBeans为创建的每个模块创建一个新的Java项目。 在最终版本中，NetBeans将允许在一个Java项目中拥有多个模块。 当使用命令提示符时，我会使用更多Java9 特有的选项相对于使用 NetBeans时。

本章介绍的程序非常简单。 当程序运行时，它打印一个消息和主类所属模块的名称。

一. 使用命令提示符

以下小节介绍使用命令提示符创建和运行第一个模块的步骤。

1. 设置目录

将使用以下目录层次结构来编写，编译，打包和运行源代码：

``````````
C:Java9RevealedC:Java9RevealedlibC:Java9RevealedmodsC:Java9RevealedsrcC:Java9Revealedsrccom.jdojo.intro
``````````

这些目录在Windows系统上设置的。 在非Windows操作系统上，你也可以设置类似的目录层次结构。 C:Java9Revealed是顶级目录，它包含三个子目录：lib，mods和src。

src目录用于保存源代码，其中包含一个com.jdojo.intro的子目录，并创建一个同名的com.jdojo.intro的模块，并将其源代码保存在这个子目录下。 在这种情况下，是否有必要将子目录命名为com.jdojo.intro？ 答案是不。 子目录����是不同的名字，或者可以将源直接存储在src目录中，而不需要com.jdojo.intro子目录。 但是，最好将目录命名为与模块名称相同的模块的源代码。 如果遵循这个命名约定，Java编译器将有一些选项可一次编译多个模块的源代码。

使用mods目录将已编译的字节码保存在展开的目录层次结构中。 如果需要，可以使用此目录中的代码运行应用程序。

在编译源代码之后，将其打包成���个���块化的JAR并将其存储在lib目录中。 可以使用模块化JAR来运行程序，也可以将模块JAR提供给需要运行程序的其他开发人员。

在本节的剩余部分会使用一个目录（如src或srccom.jdojo.intro）的相对路径。 这些相对路径是相对于C:Java9Revealed目录。 例如，src表示C:Java9Revealedsrc。 如果使用非Windows操作系统或其他目录层次结构，请进行适当的调整。

2. 编写源代码

你可以选择自���喜欢的���本编辑器（例如Windows上的记事本）来编写源代码。 首先创建一个名为com.jdojo.intro的模块。下面是模块声明的代码。

> ``````````
> // module-info.javamodule com.jdojo.intro { }
> ``````````

模块声明很简单。 它不包含模块描述语句。 将其保存在srccom.jdojo.intro目录下名为module-info.java的文件中。

然后创建一个名为Welcome的类，将它保存在com.jdojo.intro包中。 请注意，包的名字与模���具有相同的���称。 但必须保持模块和包名称相同吗？ 答案是否。 也可以选择所需的任何其他包名称。 该类将具有一个主方法public status void main(String \[\] args)。 该方法将作为应用程序的入口点。 在此方法内打印消息。

Welcome类中的打印出模块的名称。 JDK 9在java.lang包中添加了一个名为Module的类。 Module类的一个实例代表一个模块。 JDK 9中的每个Java类都是模块的成员，甚��是int，long和char等原始类型。 所有原始类型都是java.base模块的成员。 JDK 9中的Class类有一个名为getModule()的新方法，它返回该类作为其成员的模块引用。 以下代码打印了Welcome类的模块名称。

> ``````````
> Class<Welcome> cls = Welcome.class;Module mod = cls.getModule();String moduleName = mod.getName();System.out.format("Module Name: %s%n", moduleName);
> ``````````

提示：


所有原始数据类型都是java.base模块的成员。 可以使用int.class.getModule()获取int基本数据类型所属模块的引用。

下面的代码是Welcome类中的代码。并保存在名为Welcome.java的文件中，目录是comjdojointro，它是srccom.jdojo.intro目录的子目录。 此时，源代码文件的路径将如下所示：

 *  C:Java9Revealedsrccom.jdojo.intromodule-info.java
 *  C:Java9Revealedsrccom.jdojo.introcomjdojointroWelcome.java

> ``````````
> // Welcome.javapackage com.jdojo.intro;public class Welcome { public static void main(String[] args) { System.out.println("Welcome to the Module System."); // Print the module name of the Welcome class Class<Welcome> cls = Welcome.class; Module mod = cls.getModule(); String moduleName = mod.getName(); System.out.format("Module Name: %s%n", moduleName); }}
> ``````````


[Java 9_-]: http://p3.pstatp.com/large/pgc-image/15243841038602f389c1e68
 *  **原文作者：** IT民工张大川
 *  **原文链接：** https://www.toutiao.com/item/6546823196878832136/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。