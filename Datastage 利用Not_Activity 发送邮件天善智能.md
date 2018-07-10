---
title: Datastage 利用Not_Activity 发送邮件
date: 2017-03-27 18:12:06
categories: "开发"
tags:
	- 脚本语言
	- Linux
	- IBM
	- 红帽公司
	- 大数据

---

**每日干货好文分享丨请点击+关注**

欢迎关注天善智能微信公众号，我们是专注于商业智能BI，大数据，数据分析领域的垂直社区。

对商业智能BI、大数据分析挖掘、机器学习，python，R等数据领域感兴趣的同学加微信：**tstoutiao**，邀请你进入头条数据爱好者交流群，数据爱好者们都在这儿。

版本：IBM InfoSphere DataStage V11.5.1  


操作系统：linux redhat 6.4

平台：Apache Hadoop 2.6.0-cdh5.9.0

场景：

在DS调度，我们可以用DS自带的发送邮件控件去，对报错或者预警的作业来进行监控。方便及时维护或处理。

控件知识：

    使用“通知”阶段来指定关于电子邮件通知活动的信息。
    
    名为 dssendmail_template.txt 的电子邮件模板文件规定了所发送的通知电子邮件的格式。
    该文件的副本存在于每个项目目录中，例如 C:IBMInformationServerServerprojectsmyproject。 
    
    您可以编辑该文件，以便能够针对各项目使用不同的电子邮件格式。
    您可以指定参数，该参数的值由您在运行时为 SMTP 邮件服务器名称字段、发件人电子邮件地址字段、
    收件人电子邮件地址字段和电子邮件主题字段指明。 单击浏览可打开“外部参数助手”，它向您显示作业
    序列中此时可用的所有参数。在这些字段中输入的参数需要使用井号（#）来定界。从“外部参数助手”选择
    的参数会自动包含在两个井号之间。
    
    “通知”阶段包含以下字段。SMTP 邮件服务器名称服务器的名称或 IP 地址。
    您可以指定参数，其值由您在运行时指明。发件人电子邮件地址通知发送自的电子邮件地址。
    收件人电子邮件地址通知发送到的电子邮件地址。您可以指定多个以空格隔开的电子邮件地址。
    电子邮件主题包含在通知的主题行中的文本。附件要随通知一起发送的文件。
    指定一个路径名或一组以逗号分隔的路径名，它们必须包含在单引号或双引号中。
    还可以指定能解析为路径名或以逗号分隔的路径名的表达式。
    电子邮件正文包含在通知正文中的文本。
    在电子邮件中包含作业状态用来指定是否要在消息中包含可用作业状态信息的复选框。
    不对运行执行检查点操作用来指定您是否希望不记录此特定通知操作的检查点信息的复选框。
    这表示如果序列中的作业在稍后失败，并且序列已重新启动，那么无论该通知操作先前是否已成功执行，
    都将重新执行该通知操作。仅在序列作为一个整体记录检查点的情况下，此选项才可用。

前提：

首先需要申请发送邮箱服务器SMTP信息，如果发送邮件服务器不和DS在一个机器上，需要授权DS服务器有发送邮件权限。同时需要申请一个应用账号，作为邮件的发送方。同时要配置 /etc/postfix/main.cf文件中的relayhost 参数，修改成relayhost=SMTP服务器名称

说明 DatastageNotification\_Activity 控件提供发送邮件功能，对Sequence 报错发送日志。

A．开发作业如下：

![Datastage 利用Not\_Activity 发送邮件][Datastage _Not_Activity]

B．对于前一个作业执行失败报错，设置触发类型 Failed

![Datastage 利用Not\_Activity 发送邮件][Datastage _Not_Activity 1]

C．配置Notification\_Activity控件内容

![Datastage 利用Not\_Activity 发送邮件][Datastage _Not_Activity 2]

款项Include job status email 邮件内容会多增加如下信息。样例：

    STATUS REPORT FOR JOB: Seq_Dwi_Brm Generated: 2017-03-24 02:56:35  Job start time=2017-03-24 02:30:03  Job end time=2017-03-24 02:56:35  Job elapsed time=00:26:32  Job status=1 (Finished OK)  Job has no stages.

D．同时在Attachments中加载文件，该文件从DS资料库中获取

1.  开发获取资料库作业运行状态信息

![Datastage 利用Not\_Activity 发送邮件][Datastage _Not_Activity 3]

1.  该作业如下，测试环境Ds作业名：Pjob\_Dw\_Control\_email

![Datastage 利用Not\_Activity 发送邮件][Datastage _Not_Activity 4]

5 执行脚本：

注：资料库记录时间要比时间运行时间完 8 hour。

    SELECT DISTINCT H.HostName, --主机名称  J.ProjectName, --工程名  J.JobName, --作业名称  r.RUNTYPE, ---- code converted to a readable name --run为正常运行方式，VAL“仅验证”运行 RES 重置运行  R.InvocationId, --实例  R.RunStartTimeStamp + 8 hour AS RunStartTimeStamp, --该阶段启动的时间。  r.runendtimestamp + 8 hour AS runendtimestamp, --该阶段完成的时间。如果该阶段仍在运行，该列将设为空值。  S1.MajorStatusName, --运行最终状态  S2.MinorStatusName, --运行状态，详细  TotalRowsConsumed, --源阶段链接中的所有行数总计  TotalRowsProduced, --目标阶段链接中的所有行数总计  R.ElapsedRunSecs / 60 ElapsedRunSecs --阶段所运行的时间长度（以秒计）。该时间长度是根据 StageStartTimeStamp 和 StageEndTimeStamp 列计算得出的  FROM DSODB.JobExec J,  DSODB.JobRun R,  DSODB.Host H,  DSODB.JobRunParamsView P,  DSODB.RunMajorStatusRef S1,  DSODB.RunMinorStatusRef S2 WHERE H.HOSTID = J.HOSTID  AND R.JOBID = J.JOBID  AND R.RUNID = P.RUNID  --AND P.ParamName = 'paramname' AND P.ParamValue = 'paramvalue'  AND R.RunMajorStatus = S1.MajorStatusCode  AND R.RunMinorStatus = S2.MinorStatusCode  --and TotalRowsProduced <> 0  --AND J.JobName='Pjob_Stg0_T_BRM_ORDER'  --AND S2.MinorStatusName='Finished - aborted'  AND CAST(RunStartTimeStamp + 8 hour AS DATE) =  CAST(TO_DATE('#$ETL_DATE#', 'YYYYMMDD') AS DATE) ORDER BY R.RunStartTimeStamp + 8 hour desc;

对商业智能BI、大数据分析挖掘、机器学习，python，R等数据领域感兴趣同学加微信：**tstoutiao**，邀请您加入头条数据爱好者交流群，数据爱好者们都在这儿。

![Datastage 利用Not\_Activity 发送邮件][Datastage _Not_Activity 5]

本文来源自天善社区要选就选S型的博客。

原文链接：https://ask.hellobi.com/blog/Zeehom/6903。


[Datastage _Not_Activity]: /pro/os/crawler/UUAV-V2NN-RQQJ.jpg
[Datastage _Not_Activity 1]: /pro/os/crawler/QMFJ-VYQB-URMY.jpg
[Datastage _Not_Activity 2]: /pro/os/crawler/MZE3-2AIB-IN7N.jpg
[Datastage _Not_Activity 3]: /pro/os/crawler/6VYJ-MNRY-YVJA.jpg
[Datastage _Not_Activity 4]: /pro/os/crawler/7JJB-RIMU-RYQA.jpg
[Datastage _Not_Activity 5]: /pro/os/crawler/UVNF-EJUM-MMNR.jpg
 *  **原文作者：** 天善智能
 *  **原文链接：** https://www.toutiao.com/item/6402119165141844481/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。