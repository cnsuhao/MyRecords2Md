---
title: SpringBatch-组合写，复杂业务逻辑批处理必备「干货」
date: 2017-09-19 22:21:52
categories: "开发"
tags:
	- Java
	- 技术
	- XML

---

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch-]

# 适用版本 #

1.  SpringBatch 3.0.8
2.  Spring4.3

# SpringBatch面临的现实问题 #

SpringBatch框架的批处理能力毋庸置疑，java批处理新规范JSR-352即是受其深度影响。但是其官方提供的API在实际运用中还是收到了挑战，比如在实际场景中一个任务(Step)会更新多张表，而SpringBatch官方提供的API一个Step只能更新一张表。可能有人会说可以在Processor中更新数据，不用全部在ItemWriter中更新数据，没错，这虽然能解决功能性上的问题，但破坏了SpringBatch的数据处理模型：ItemReader->Processor->ItemWriter，且性能上也不会好，要知道Processor中每次拿到的只是“一条数据”，在大数据量的更新操作上，使用不到JDBC的Batch功能。

# 复合写功能实现 #

MyBatis官方提供了组合写人ItemWriter:org.springframework.batch.item.support.CompositeItemWriter，但是很遺憾，使用起来并不方便，主要体现在参数的传递上。

# 场景 #

复合写要解决的应用场景是一个Step更新一张主张，多张从表、子表。模型如下：

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch- 1]

模型

**模型对象示例代码:** 

可以有多张从表，多张子表，这里是示意，只定义了一个。

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch- 2]

JavaBean定义

# **关键点** #

通过SpringBatch数据处理模型：ItemReader ==》 Processor ==》ItemWriter可以看到，T是我们的业务逻辑处理对象，O是更新数据的对象。对于组合写场景，O必然是一个复杂对象，不会是简单的JavaBean。而且组合写中的多个ItemWriter拿到的对象必然不一样。

MyBatis提供的示例中缺乏对于上述模型的支持。

**MyBatisComplexItemWriter实现**

这里的ItemWriter与一般的Writer不同之处是可以获取模型中取得当前wirter需要处理的数据。

先给出XML配置, compositetWriters配置了两张表的数据写入操作，T\_DESTCREDIT（主表）、T\_TRADE\_RECORD（子表）。MyBatisComplexItemWriter是自定义类，主要作用是从主模型中取得当前ItemWriter需要的数据，并做校验，然后将实际的数据写入操作委派给MyBatisBatchItemWriter。ttradeRecordWriter这个bean的配置即是标准的MyBatis的ItemWriter配置，不再赘述。

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch- 3]

itemWriter配置

**MyBatisComplexItemWriter实现**

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch- 4]

**MyBatisComplexItemWriter（1）**

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch- 5]

**MyBatisComplexItemWriter（2）**

![SpringBatch-组合写，复杂业务逻辑批处理必备「干货」][SpringBatch- 6]

**MyBatisComplexItemWriter（3）**

更多java开发请关注我的头条，完整请示例私信我！  



[SpringBatch-]: /pro/os/crawler/FEQV-ZI7B-IRJN.jpg
[SpringBatch- 1]: /pro/os/crawler/QAA2-MFU7-BBYU.jpg
[SpringBatch- 2]: /pro/os/crawler/UIRR-MJVZ-2EYR.jpg
[SpringBatch- 3]: /pro/os/crawler/FMNR-VBM6-JM6B.jpg
[SpringBatch- 4]: /pro/os/crawler/VABY-QYRY-67BR.jpg
[SpringBatch- 5]: /pro/os/crawler/3E2A-RIZQ-MZFR.jpg
[SpringBatch- 6]: /pro/os/crawler/MEMZ-AM6F-QQEE.jpg
 *  **原文作者：** 用Java打酱油
 *  **原文链接：** https://www.toutiao.com/item/6467494519620239886/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。