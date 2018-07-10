---
title: MyBatis-从查询昨天的数据说起
date: 2017-08-09 23:07:35
categories: "开发"
tags:
	- SQL

---

前段时间写了《RabbitMQ入门》系列

 *  RabbitMQ入门-初识RabbitMQ
 *  RabbitMQ入门-从HelloWorld开始
 *  RabbitMQ入门-高效的Work模式
 *  RabbitMQ入门-消息派发那些事儿
 *  RabbitMQ入门-消息订阅模式
 *  RabbitMQ入门-Routing直连模式
 *  RabbitMQ入门-Topic模式

主要讲了一些RabbitMQ的基本知识点，后面准备再起个Spring集成RabbitMQ系列，希望能够更加贴近我们的日常接触的生产环境。  


今天这篇跟Mybatis以及sql语句有关，正好也是今天解决问题的实践总结。  


# 流水统计    #

**业务背景**  


做一个流水统计的功能，从流水明细表中，每天定时同步前一天的流水，按照两个以上的维度统计并存储到新的统计表中。  


对于明细表中过时的数据需要清除以防止明细表的无限增长。

**设计思路**

因为项目使用的是Mybatis，所以需要在这个框架下实现想要的功能。抛开框架，问题比较简单，最核心的其实是sql语句。

但是坦白说，sql语句一直也就是简单的使用，尤其是现如今有以Hibernate等为代表ORM框架，我们很少需要手写那些sql语句，甚至在一些成熟的产品项目里，sql语句更是难得一见。

整个思路很简单，我需要定义一个定时器，这个可以使用Spring Quartz来配置，其中还涉及到Cron expression即定时器表达式。然后我们要在Mybatis的壳子里塞入我们需要的sql语句。最后待时间点一到就执行sql语句完成流水统计，就大功告成了。下面罗列些解决这个问题的点。

# Mybatis    #

个人认为Mybatis是灵活的，但同时也是繁琐的。为了写上一个我们想要执行的sql语句，需要写一大堆的声明代码。  


有的sql语句有输入参数比如where后的比较条件就涉及到参数，这时候在Mybatis就要提供输入参数的入口，我们可以用parameterType来定义你想要的输入参数。相应的，执行完sql语句有时候会有返回结果，比如select完后的结果，这时候我们可以通过resultMap来返回，必要的时候你需要定义一个resultMap，好比下面这样

![MyBatis-从查询昨天的数据说起][MyBatis-]

这实际上是一种映射，将数据库字段的identity\_card\_id与Model中的identityCardId对应起来。

对于我们的问题来说，需要首先从明细表中查出所有符合条件的流水明细记录，然后将符合条件的记录统计并插入到统计表中。Mybatis提供了增删改查相应的声明标签<insert>、<delete>、<update>、<select>，需要执行的sql语句可以放在对应的标签中。

# 如何查询昨天的数据    #

在解决查询昨天的数据这个问题之前，我们首先得知道怎么获取今天的日期。

**SYSDATE()**

通过SYSDATE()我们可以获得当前时间，如果你经常用sql语句，应该还知道有一个now()函数，两个都是可以查到当前的时间，但是区别在于now()一旦执行后就不变了，而SYSDATE()每次执行都是当前的时间。

**DATE\_FORMAT**

有了SYSDATE()我们确实可以拿到当前时间了，那么怎么才能得到我们想要的时间格式呢，众所周知，时间的表示法千千万，比如20170809，2017-08-09等等。  


这时候我们需要使用DATE\_FORMAT()得到我们想要的日期格式比如DATE\_FORMAT(SYSDATE(), '%Y-%m-%d')执行完后，我们就得到了结果“2017-08-09”。有关DATE\_FORMAT中的第二个参数可以选择的值如下

![MyBatis-从查询昨天的数据说起][MyBatis- 1]

![MyBatis-从查询昨天的数据说起][MyBatis- 2]

**DATE\_SUB**  


有了格式化的DATE\_FORMAT函数，我们可以得到想要的日期格式，有了SYSDATE()也能够得到今天的具体时间了，那么如何得到昨天，明天的时间呢，如果这步可以实现，那么离我们统计昨天所有流水明细的任务就不远了。

这时候我们可以用DATE\_SUB来解决，比如date\_sub(SYSDATE(), interval 1 day)表示在当前时间的基础上往前提一天就是昨天。当然，我们也可以使用DATE\_ADD把日期调到明天。

有了这些sql的函数，我们已经可以实现预期的功能了。最终的sql语句类似

![MyBatis-从查询昨天的数据说起][MyBatis- 3]

有了这些知识点，对于上面提到的定期删除数据以及其他的数据整理工作基本上都能解决了，剩下的就是敲代码实现业务了。


[MyBatis-]: /pro/os/crawler/7JZ7-F3MJ-EV3Q.jpg
[MyBatis- 1]: /pro/os/crawler/ZMZ6-V37J-EVUI.jpg
[MyBatis- 2]: /pro/os/crawler/77ZN-7ZVV-VNYR.jpg
[MyBatis- 3]: /pro/os/crawler/AJFM-QFNN-NYVY.jpg
 *  **原文作者：** JackieZheng
 *  **原文链接：** https://www.toutiao.com/item/6452291807827984910/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。