---
title: idea 编译报错 javacTask 源发行版1.8 需要目标发行版1.8
date: 2017-04-30 21:34:56
categories: "开发"
tags:
	- java
	- idea

---

# 错误 #

在Idea2017.1中编译时发生如下的错误

``````````
Information:java: javacTask: 源发行版 1.8 需要目标发行版 1.8
Information:java: Errors occurred while compiling module 'suanfa'
Information:javac 1.8.0_111 was used to compile java sources
Information:Module "suanfa" was fully rebuilt due to project configuration/dependencies changes
Information:2017/4/30 下午9:27 - Compilation completed with 1 error and 0 warnings in 1s 547ms
Error:java: Compilation failed: internal java compiler error
``````````

# 解决 #

perferences -> Build,Execution, Deployment -> Compiler -> Java Compiler
设置相应Module的 bytecode version即可
![这里写图片描述][2E73-IQBQ-ZZ3E.jpg]

# 参考 #

[Idea 编译报错 javacTask: 源发行版 1.6 需要目标发行版 1.6
][Idea _ javacTask_ _ 1.6 _ 1.6]

# MAVEN项目 （2017-11-17） #

如果你用maven构建的项目，那么在pom.xml中添加编译插件，关指明编译器的原/目标版本，ideal会自动给你配制编译器的版本，无需手动设置了，非常的贴心和智能。

``````````
<build>
        <plugins>
            <!--compiler插件-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>${jdk.version}</source>
                    <target>${jdk.version}</target>
                    <encoding>${file.encoding}</encoding>
                </configuration>
            </plugin>         
        </plugins>
    <build>
``````````


[2E73-IQBQ-ZZ3E.jpg]: /pro/os/crawler/2E73-IQBQ-ZZ3E.jpg
[Idea _ javacTask_ _ 1.6 _ 1.6]: https://bigjin.com/archives/idea-javacTask.html
 *  **原文作者：** weixin_36210698
 *  **原文链接：** https://blog.csdn.net/weixin_36210698/article/details/71036504?locationNum=6&fps=1
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。