---
title: IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6
date: 2018-05-11 15:20:55
categories: "开发"
tags:
	- 职场
	- Excel
	- PowerPoint

---

在企业的业务数据数据分析过程中，经常会遇到类似Adhoc形成的数据分析请求，这些adhoc请求因为其需要根据不同业务背景向目标用户提供特殊数据分析结果和管理报告，因此传统的BI架构模式因为其设计上的约束较难“因需而动”。而轻量化BI解决思路因为其没有严格ETL和业务建模的定义约束，用户在使用上会比较容易上手并且企业数据存储形式可能存在多种形式。在很多时候IT面对的数据是呈“碎片化”形式，业务人员提供的原始基础数据往往是Excel 格式，客观上需要一个敏捷化分析工具能完成这些数据的聚合工作。

> ——本案例基于IT服务质量进行分析，所用的开发环境为FineBI V4.1

# 一、需求背景 #

Tea公司IT部门使用ITIL V3模式管理公司的IT运维业务。每天业务用户系统使用问题通过Helpdesk上报并分配到相关的IT团队进行处理。按照ITIL要求对每个IT事件定义了4个等级（P1 –P4），每个等级对应不同的服务等级，例如P1 事件需要在2小时内关闭。Tea公司IT运维事件有3个服务团队支持，本地系统有所在国IT团队负责，集团业务系统有集团IT负责处理，外包系统有第三方团队支持处理。目前Helpdesk 已经记录所有IT服务事件运维情况，并在每月提供一份Excel 原始数据给IT 服务经理分析服务质量。原先服务质量分析基于Excel 数据分析效率低，因此该服务经理希望使用BI产品完成Helpdesk服务事件的多维分析。基本需求如下：

① 按事件等级、IT团队、时间等维度统计Incidents 的数量并能完成数据的穿透，评估IT各团队的工作负荷

② Helpdesk 对时间完成情况有完整的状态记录，因此需要按事件不同状态（open、closed、resolved）统计目前服务事件的分布情况（按事件状态、支持团队等维度能实现数据展开）

③ 需要按时间维度对反映服务事件的趋势变化洞悉IT服务质量变化情况

④ 目前外包方的服务质量需要改善，因此需要按外包商单独进行主题分析

⑤ 最终用户需要提供数据联动、数据下钻和数据切片自主操作功能

⑥ 数据展现需要容易业务用户理解并能通过BI分析自主发现服务过程中存在问题并提出改进措施

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6]

上述Excel文件Helpdesk 每月提供一份到IT 服务部门。

**a、通过分析整理出上述表单的字段业务定义如下：**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 1]

b、通过调研发现，目前IT 服务经理在PPT汇报中将所有支持团队分为CN、Global team、PMTeam、Vendor 作为分析大主题或高层聚合维度进行图表或者Pivot的展示。这些维度映射信息在原始数据上是没有的，是通过建立映射表将原始字段中的“AssignedTo Team”完成lookup table的构建。该映射表关系如下：

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 2]

**2.2 FineBI 数据建模**

基于上述收集的业务数据，Tea公司BI团队 决定使用FineBI（轻量化BI工具）完成这样业务需求。

首先，根据业务原始数据，在FineBI中创建数据包（包名“ITIL”），内部有2个业务表单：IM ticket（Helpdesk服务日志）和DIM\_Support\_Team（团队映射表）。

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 3]

其次，将原始IM ticket.xls 通过ETL 装载到FineBI的ITIL业务包中，为了便于理解将原字段名按如下图所示进行转义命名。

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 4]

将团队映射表构建完成后，装载到ITIL业务包中，如下图所示。

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 5]

最终ITIL 业务包：

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 6]

再次:因为报表需要按团队进行聚合，因此需要在DIM\_Support\_Team 同IM ticket 业务包构建关系。 关键字如下DIM\_Support\_Team->TeamName vs IM Ticket->Assigned team name (1:N),构建关系如下图所示：

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 7]

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 8]

# 三、BI模板设计 #

**3.1 业务指标规划**

在构建完服务质量分析的数据环境后，更新完FineIndex 后，Tea BI 团队就开始BI前端展示页面的开发。FineBI 提供丰富的图表控件用于前端展示，但是需要Tea BI 团队根据业务需求进行数据展现逻辑的定义。需要对业务分析所需的维度和相关的聚合指标进行梳理。经过分析，关于服务质量分析维度规划如下：

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 9]

初步规划统计指标定义如下：

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 10]

在本分析项目原型阶段，Tea BI团队需要按需求开发如下业务分析模板：

**3.2 服务工作量分析**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 11]

**业务用途：**

基于服务事件的统计每个团队、每种优先级的事件、每个月度的服务事件数量。用于整体评估采用ITIL 模式后IT运维的工作量。

**设计说明：**

**1、按服务等级统计工作量**

图表元件： 圆环仪表盘

图表分类：IM ticket -> 服务等级

图表指标：IM ticket ->IM ticket记录数

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 12]

**2、服务类型分布**

图表元件： 多层饼图

图表分类：IM ticket -> 服务属性大类

IM ticket ->服务等级（分组依据按相同值）

IM ticket ->所属支持团队

图表指标：IM ticket ->IM ticket记录数

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 13]

**3、按服务项目统计工作量**

图表元件： 词云图

词名：IM ticket -> 服务项目

指标：IM ticket ->IM ticket记录数

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 14]

**4、按BU统计各服务团队服务工作量**

图表元件： 堆积条形图

分类：IM ticket -> 所属BU

系列： IM ticket -> 所属支持团队

指标：IM ticket ->IM ticket记录数

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 15]

**5、按服务分类统计服务事件分布**

图表元件： 交叉表

行表头：IM ticket -> 服务所属大类

IM ticket -> 服务所属中类

IM ticket -> 服务所属小类

列表头： IM ticket -> 建立时间（月份）

指标：IM ticket ->IM ticket记录数

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 16]

**6、月度服务事件统计**

图表元件： 组合图

分类： IM ticket -> 建立时间（月份）

系列： IM ticket ->服务等级

左值轴：IM ticket ->IM ticket记录数

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 17]

**7、服务数据明细表**

图表元件： 明细表

数据：

IM ticket -> 事件编号

IM ticket -> 事件描述

IM ticket -> 服务项目

IM ticket -> 事件状态

IM ticket -> 事件负责人

IM ticket -> 所属团队

IM ticket -> 事件发起者

IM ticket -> 建立时间

IM ticket -> 最后更新时间

IM ticket -> 解决时间

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 18]

**3.3 服务绩效分析**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 19]

**业务用途：**

基于服务事件的统计每个团队、每种优先级的事件、每个月度的服务事件完成情况。用于整体评估采用ITIL 模式后IT运维绩效。

**设计说明：**

**1、团队服务完成率分析**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 20]

**2、服务事件趋势分析**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 21]

**3、团队服务占比分析**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 22]

**计算列公式定义参照如下图**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 23]

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 24]

**3.4 外包服务分析**

**业务用途：**

基于服务事件的统计外包服务团队服务事件完成情况。用于整体评估采用ITIL 模式后外包运维绩效。

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 25]

**设计说明：**

**1、外包服务质量分析**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 26]

**计算列定义如下**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 27]

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 28]

**2、外包事件完成率**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 29]

**3、明细表**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 30]

# 四、应用成果 #

**服务工作量分析：**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 31]

**服务工绩效分析：**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 32]

**外包厂商服务质量分析：**

![IT运维效率提高32%！用FineBI多维分析Helpdesk服务，这一招很6][IT_32_FineBI_Helpdesk_6 33]

Tea BI团队通过FineBI 分析工具快速搭建完成原型分析模板的测试原型，并在测试环境中完成发布供服务经理确认满足其业务分析要求。改变了以往通过信息部提供一份Excel原始数据给IT服务经理分析服务质量这种低效率的数据分析模式成功实现服务经理应用FineBI产品完成Helpdesk服务事件的多维分析过程。

经过一段时间的数据分析和业务决策追踪引导，针对低质量的服务问题进行追责和优化整改，最终Tea公司的团队服务质量水平同比提高了32.57%，Tea BI团队较为快速的业务需求响应，圆满完成这项BI分析和决策引导优化提升服务质量任务。


[IT_32_FineBI_Helpdesk_6]: /pro/os/crawler/UFN3-IUVE-B2YJ.jpg
[IT_32_FineBI_Helpdesk_6 1]: /pro/os/crawler/B6NA-VVIJ-ENZJ.jpg
[IT_32_FineBI_Helpdesk_6 2]: /pro/os/crawler/ANBE-3MRJ-ERJB.jpg
[IT_32_FineBI_Helpdesk_6 3]: /pro/os/crawler/YYM2-MVBI-RJEJ.jpg
[IT_32_FineBI_Helpdesk_6 4]: /pro/os/crawler/QUFN-BBBI-MNAA.jpg
[IT_32_FineBI_Helpdesk_6 5]: /pro/os/crawler/FFYM-EE6R-IEFY.jpg
[IT_32_FineBI_Helpdesk_6 6]: /pro/os/crawler/NAAE-2EBF-BERZ.jpg
[IT_32_FineBI_Helpdesk_6 7]: /pro/os/crawler/VJYM-RFFJ-6VVU.jpg
[IT_32_FineBI_Helpdesk_6 8]: /pro/os/crawler/FQVE-B3EI-MMUV.jpg
[IT_32_FineBI_Helpdesk_6 9]: /pro/os/crawler/NN2M-AA3I-RUQB.jpg
[IT_32_FineBI_Helpdesk_6 10]: /pro/os/crawler/EZM7-VNI2-2EEV.jpg
[IT_32_FineBI_Helpdesk_6 11]: /pro/os/crawler/U3Q7-NQZI-BZ3U.jpg
[IT_32_FineBI_Helpdesk_6 12]: /pro/os/crawler/QN77-RRYN-MIBV.jpg
[IT_32_FineBI_Helpdesk_6 13]: /pro/os/crawler/IMYZ-JYZ7-RUYQ.jpg
[IT_32_FineBI_Helpdesk_6 14]: /pro/os/crawler/QYBA-MN2M-U2EQ.jpg
[IT_32_FineBI_Helpdesk_6 15]: /pro/os/crawler/EBFI-NJJA-7FAE.jpg
[IT_32_FineBI_Helpdesk_6 16]: /pro/os/crawler/ANBJ-NMIR-JZMN.jpg
[IT_32_FineBI_Helpdesk_6 17]: /pro/os/crawler/EN6N-ERM7-3AFB.jpg
[IT_32_FineBI_Helpdesk_6 18]: /pro/os/crawler/NRQV-7VYR-ZUJU.jpg
[IT_32_FineBI_Helpdesk_6 19]: /pro/os/crawler/MRA3-QBMN-IYAM.jpg
[IT_32_FineBI_Helpdesk_6 20]: /pro/os/crawler/YEQY-INIZ-ZAI3.jpg
[IT_32_FineBI_Helpdesk_6 21]: /pro/os/crawler/FEQZ-V3VY-EAQN.jpg
[IT_32_FineBI_Helpdesk_6 22]: /pro/os/crawler/FEQA-YIVR-3EQ2.jpg
[IT_32_FineBI_Helpdesk_6 23]: /pro/os/crawler/BZA6-BE3I-QEAU.jpg
[IT_32_FineBI_Helpdesk_6 24]: /pro/os/crawler/RQBE-U36Z-IUYJ.jpg
[IT_32_FineBI_Helpdesk_6 25]: /pro/os/crawler/YUNA-NQVQ-IVRV.jpg
[IT_32_FineBI_Helpdesk_6 26]: /pro/os/crawler/BQVJ-M2B3-ENMM.jpg
[IT_32_FineBI_Helpdesk_6 27]: /pro/os/crawler/B222-63RV-Y2YR.jpg
[IT_32_FineBI_Helpdesk_6 28]: /pro/os/crawler/I3UA-JYUU-YBFY.jpg
[IT_32_FineBI_Helpdesk_6 29]: /pro/os/crawler/BUB2-YBMY-3I6R.jpg
[IT_32_FineBI_Helpdesk_6 30]: /pro/os/crawler/RVAE-V3NZ-QB3A.jpg
[IT_32_FineBI_Helpdesk_6 31]: /pro/os/crawler/QMRU-RBMB-VANR.jpg
[IT_32_FineBI_Helpdesk_6 32]: /pro/os/crawler/UNEZ-JZNQ-NZU2.jpg
[IT_32_FineBI_Helpdesk_6 33]: /pro/os/crawler/RYAI-JMEQ-IRMI.jpg
 *  **原文作者：** 帆软
 *  **原文链接：** https://www.toutiao.com/item/6554219930630226445/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。