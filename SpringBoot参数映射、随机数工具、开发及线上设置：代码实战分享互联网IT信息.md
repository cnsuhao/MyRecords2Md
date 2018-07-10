---
title: SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享
date: 2018-06-07 17:35:00
categories: "开发"
tags:
	- 技术
	- MySQL
	- 人工智能
	- 大数据
	- 云计算

---

# **提示：目前整理的springboot的实战代码，可通过以下方式获得下载方式：** #

关注头条号“互联网IT信息”——>私信发送 “参数映射” ，即可获取代码下载的方式。

**同时为了感谢大家一直以来的支持，私信后��可以获取如下架构资料：**

人工智能、高端架构、大数据、云计算、分布式、微服务

# 1. SpringBoot的自定义参数映射 #

**（1）自定义参数映射简介：**

springboot整体是提倡是用较少的配置文件，如果有些参数，你不得不通过配置文件进行设置，以增加系统线上使用的灵活性。

**（2）springboot进行自定义参数映射的核心步骤简介：**

1）创建springboot的基础工程

2）增加配置文件���并根据业务需要自定义配置文件中的参数值

3）创建自定义参数对应的实体类

4）利用springboot的启动类，进行测试参数的获取

**（3）自定义参数映射的步骤代码实现如下：**

**1）创建springboot的基础工程**

第一：基于maven创建一个工程，工程名为springbootproper

第二步：修改maven工程的pom.xml文件，核心内容如下：

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战��享][SpringBoot]

**2）增加配置文件，并根据业务需要自定义配置文件中的参数值**

在工程的resources目录下创建配置文件application.properties，里边内容如下：

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享][SpringBoot 1]

**3）创建自定义参数对应的实体类**

该实体类的目的是：针对映射配置文件中的参数设置，然后通过代码就可以获取参数值。

其中该实体类有个���解：@ConfigurationProperties(prefix = "other")

含义是：针对application.properties配置文件中以other开头的参数，其他开头的不去映射

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享][SpringBoot 2]

**4）利用springboot的启动类，进行测试参数的获取**

创建包com.gongyunit.proper.springboot，并在该包下创建springboot的启动类：

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享][SpringBoot 3]

# 2. SpringBoot的自定义属性中的随机数工具类 #

springboot为配置参数提供随机数工具类，该工具类可以直接在配置文件中使用，常用的随机数方法简介如下：

random.long：一个随机long类型数据

random.int：一个随机int类型shuju

random.uuid：一个随机的uuid

random.int\[1，200\]：从1至200之间取随机数

random.value：随机的一个字符串

补充：随机数工具类方法如何在代码中应用，下边会结合多环境配置一块讲解

# 3. springboot开发及线上等多环境设置 #

在实际项目中，可能存在研发人员的开发环境和线上参数配置不一致的情况，为了让研发人员能灵活的在多种情况下切换配置参数，springboot提供了一种多环境配置的方式，具体讲解如下：

**（1）springboot将配置文件分成1+N个文件**

1是指：主配置文件，里边核心定义选取N中的哪个文件，命名为：application.yml，里边的核心内容如下：

N个文件的取名规则是：application-xxx.yml,比如我们这去两个文件，名称分别为：

application-dev.yml

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享][SpringBoot 4]

application-prod.yml

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享][SpringBoot 5]

**（2）application.yml文件中active这个属性写什么，决定了项目中实际应用哪个文件，比如这里我们写dev，就是项目中会使用application-dev.yml中的文件内容**

**（3）基于配置文件，编写相应的参数映射实体类，用来获取参数中的值**

该实体类为：SystemProperties

![SpringBoot参数映射、随机数工具、开发及线上设置：代码实战分享][SpringBoot 6]

**（4）在springboot的启动类中进行测试，启动类修改为如下内容：**

![SpringBoot参数映射、随机数工具、开发及线上设置：���码实战分享][SpringBoot 7]

**测试结果是：项目一启动就会有如下打印内容：**

SystemProperties\{internalTime='-8990625280794778035', machineId='502bfb96-529f-4fb7-803f-b916cb9c864a', database='mysql', sumup='机器的标识：e2dfbdb2-4a0f-4e33-9a33-0a5d7da984b5，用的数据库是：mysql'\}

# **再次提醒：目前整理的springboot的实战代码，可通过以下方式获得下载方式：** #

关注头条号“互联网IT信息”——>私信发送 “参数��射” ，即可获取代码下载的方式。

**同时为了感谢大家一直以来的支持，私信后也可以获取如下架构资料：**

人工智能、高端架构、大数据、云计算、分布式、微服务


[SpringBoot]: /pro/os/crawler/QFFJ-YYIV-7BBU.jpg
[SpringBoot 1]: /pro/os/crawler/2EYM-Y2II-JEBU.jpg
[SpringBoot 2]: /pro/os/crawler/JIAF-Z322-2AI3.jpg
[SpringBoot 3]: /pro/os/crawler/Q2UF-EFNB-Z22E.jpg
[SpringBoot 4]: /pro/os/crawler/MRQU-JBAN-FQRM.jpg
[SpringBoot 5]: /pro/os/crawler/Z7BQ-JQYJ-Z67J.jpg
[SpringBoot 6]: /pro/os/crawler/REZM-QIVB-MUNF.jpg
[SpringBoot 7]: /pro/os/crawler/YZEM-FFEZ-IQZJ.jpg
 *  **原文作者：** 互联网IT信息
 *  **原文链接：** https://www.toutiao.com/item/6564140520291959300/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。