---
title: 使用maven插件生成项目的api文档
date: 2018-02-06 13:46:45
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言
	- HTML

---

# 导读 #

在进行Java学习的时候，相信大家都看过在线或者下载的java api文档，可能是html格式或者chm格式的，其实这些参考文档也是很容易生成的，这里介绍一个maven的插件来实现项目代码文档的生成。

![使用maven插件生成项目的api文档][maven_api]

JDK 7 API

# 操作步骤 #

1. 在项目的pom.xml文件中，添加如下代码：


> <profiles>
> 
> <profile>
> 
> <id>doclint-java8-disable</id>
> 
> <activation>
> 
> <jdk>\[1.8,)</jdk>
> 
> </activation>
> 
> <build>
> 
> <plugins>
> 
> <plugin>
> 
> <groupId>org.apache.maven.plugins</groupId>
> 
> <artifactId>maven-javadoc-plugin</artifactId>
> 
> <configuration>
> 
> <additionalparam>-Xdoclint:none</additionalparam>
> 
> </configuration>
> 
> </plugin>
> 
> </plugins>
> 
> </build>
> 
> </profile>
> 
> </profiles>

**注意事项:**

（1）jdk8以后的版本添加了doclint，这个工具会规范HTML文档，对于不正确的嵌套，非法的html属性，未关闭的标签等，java doc就会生成失败，所以一个临时解决方案是将doclint在jdk8中disable掉（-Xdoclint:none）

（2）在拷贝上述配置到pom文件中时，注意标签的嵌套。譬如：在已有项目中已经存在了profiles标签，那么只需拷贝profile之间的内容即可

2. 使用maven命令生成java文档

> mvn javadoc:javadoc
> 

![使用maven插件生成项目的api文档][maven_api 1]

生成文档命令

![使用maven插件生成项目的api文档][maven_api 2]

控制台日志输出

3. 生成的html文档，会散落在各个功能模块的target/site/apidocs目录下，双击index.html即可查看生成的文档

![使用maven插件生成项目的api文档][maven_api 3]

生成的文档

# 其他  #

1. 如果不喜欢看html格式的java文档，可以到网上找工具将html合成chm格式的电子书，方便分发和携带

2. 如果在生成java文档时，报出错误“**编码GBK的不可映射字符**”，可以在pom.xml文件中设置编码格式来解决：

> <properties>
> 
> <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
> 
> <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
> 
> </properties>


[maven_api]: /pro/os/crawler/VFNY-RYFU-BVNB.jpg
[maven_api 1]: /pro/os/crawler/Q7ZN-INVI-QQAA.jpg
[maven_api 2]: /pro/os/crawler/MMF7-NUUA-YZRQ.jpg
[maven_api 3]: /pro/os/crawler/7VUY-6RVJ-RIAY.jpg
 *  **原文作者：** 京京肚肚撸代码
 *  **原文链接：** https://www.toutiao.com/item/6519313701487510019/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。