---
title: 使用RESTful接口对Elasticsearch数据进行CURD操作
date: 2017-11-15 13:51:43
categories: "开发"
tags:
	- 脚本语言
	- ElasticSearch
	- JSON
	- Windows
	- 大数据

---

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD]

大数据

> 前篇已经介绍了关于Elasticsearch在Windows下如何安装，以及介绍了Elasticsearch-head插件的安装介绍，现在就如何使用Elasticsearch来存储、管理自己的数据进行简单的介绍。本次通过使用Elasticsearch服务提供的RESTful接口进行简单使用。  
> 

# RESTful #

简单说明一下RESTful，它一种软件架构风格而不是标准，只是提供了一组设计原则和约束条件，在 REST 样式的 Web 服务中，每个资源都有一个地址。资源本身都是方法调用的目标，方法列表对所有资源都是一样的。这些方法都是标准方法，包括 HTTP GET、POST、PUT、DELETE，还可能包括 HEADER 和 OPTIONS。

下面开始讲解如使用RESTful接口对Elasticsearch数据进行CURD操作，首先启动Elasticsearch-head插件服务（具体如何安装及使用请参考前篇文章《[在Windows下安装Elasticsearch——开启大数据分析之旅][Windows_Elasticsearch]》的最后说明），访问http://127.0.0.1:9100，进入管理页面，如下图所示：

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 1]

Elasticsearch-Head控制页面

该Elasticsearch集群只有一个节点，现在只作为演示使用，一般情况下都是有多个节点构成一个集群，提高系统的健壮性。下面就依靠此工具进行介绍如何进行数据的管理。

# 创建索引    #

在Elasticsearch-Head控制面板点击“索引”标签，然后再点击“新建索引”按钮，即可弹出新建索引对话框，输入索引名称，然后点击“OK”按钮就创建了一个索引。创建成功后返回如下的JSON字符串“\{"acknowledged":true,"shards\_acknowledged":true,"index":"user"\}”就表示创建索引成功。

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 2]

创建索引

但当前所创建的索引是一个无结构的索引，即索引的映射（mappings）值为空，可在概览页面点击索引信息进行查看，如下图所示。

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 3]

无结构Elasticsearch索引

如何给当前索引添加字段，使之变成一个有结构的索引呢？打开复合查询标签页，在左侧查询中的地址栏输入服务地址，然后再下面输入要创建索引和结构的信息，选择“POST”方式进行提交，如下图所示。

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 4]

创建结构化的索引

这样当前user索引中就创建了一个类别名为type1的分类，该分类中添加了name、age和birthday三个字段，这样当前索引就是一个有结构的索引。至此索引的基本创建就完成了，接下来将介绍如何使用Elasticsearch的RESTful接口来完成数据的增删改查操作。

# 插入数据 #

数据插入有两种方式，一种是指定ID的方式插入，另一种是自动生成ID的方式，下面分别用代码来进行详细的说明。先说使用指定ID的方式，请求地址：http://127.0.0.1:9200/user/type1/1，请求方式为PUT，参数为JSON数据 \{"name": "夏风飞舞", "age": 25, "birthday": "1992-06-20" \}，点击提交就新增了一条数据，请求结构如下图所示。

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 5]

新增数据

由新增数据的请求地址可以大致看出，其中“127.0.0.1”为Elasticsearch服务器地址，“9200”为端口号，“user”为索引，“type1”为索引中的类别，最后的“1”为创建数据文档的ID。另外一种方式是去掉类型后的ID，将请求方式改成POST，请求JSON参数不变。执行完上面的操作后，点击数据浏览就可以查看到自己新增的数据了，如下图所示。

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 6]

查看数据

# 修改数据 #

修改数据也有多种方式，下面就简单介绍其中两种，这两种方式的请求地址及方式都是一样的，请求地址为http://127.0.0.1:9200/user/type1/1/\_update，请求方式为POST。第一种请求的参数为：\{ "doc": \{ "name": "夏风", "birthday": "1992-03-01" \} \}，创建一个JSON对象，将要修改的数据放入“doc”字段内，提交请求；另外一种是通过脚本进行修改，脚本JSON代码为：\{ "script":\{ "lang":"painless", "inline": "ctx.\_source.age=params.age", "params": \{ "age": 53 \} \} \}，其中语言（lang）为服务内置语言“painless”,执行脚本为“ctx.\_source.age=params.age”，字段“params”为请求的数据参数，ctx.\_source为当前doc对象。

# 删除数据 #

删除数据有两种含义，一个是删除文档（一条记录），还有就是删除整个索引（index）。删除索引是一个很危险的操作，它将会删除当前索引的所有数据。删除z指定文档请求地址为http://127.0.0.1:9200/user/type1/1，请求方式为DELETE；删除索引请求地址为http://127.0.0.1:9200/user，请求方式同样为DELETE。

# 查询数据 #

使用Elasticsearch主要就是为了查询数据，查询数据的方式也是最多的，形式和返回结果也是多种多样。先介绍一个简单的，通过指定索引、类型和文档ID来查询，请求地址为http://127.0.0.1:9200/user/type1/1，请求方式为GET，点击“提交请求”，将返回查询到的数据（JSON格式），如下图所示。

![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 7]

通过指定ID查询数据

也可以通过使用Elasticsearch-head插件控制面进行查询操作，点击基础查询，选择检索条件，并勾选“显示查询语句”，点击“搜索”按钮即可查询，同时显示出查询条件（JSON形式数据），如下图所示。  


![使用RESTful接口对Elasticsearch数据进行CURD操作][RESTful_Elasticsearch_CURD 8]

通过基础查询面板查询

查询语句简单说明，除了bool联合查询外，还有match、match\_phrase、multi\_match以及term等查询方式，他们具体的使用方式可查看官方文档，文档地址为：https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html，有机会也会发一篇关于查询的文章。

至此Elasticsearch的简单CURD操作就介绍完了，接下来会继续讲解Elasticsearch，如何结合JavaApi整合到Java项目中进行开发，敬请关注~


[RESTful_Elasticsearch_CURD]: /pro/os/crawler/J7RQ-MRNB-JFIZ.jpg
[Windows_Elasticsearch]: http://m.toutiao.com/i6488142342170608141/?group_id=6488142342170608141&amp;group_flags=0
[RESTful_Elasticsearch_CURD 1]: /pro/os/crawler/BBIN-EQ6V-QIVQ.jpg
[RESTful_Elasticsearch_CURD 2]: /pro/os/crawler/QEUU-QBFM-E6ZB.jpg
[RESTful_Elasticsearch_CURD 3]: /pro/os/crawler/FUYF-AUMB-VJAY.jpg
[RESTful_Elasticsearch_CURD 4]: /pro/os/crawler/URBY-BJJQ-NQ73.jpg
[RESTful_Elasticsearch_CURD 5]: /pro/os/crawler/AVNN-E2VM-MURV.jpg
[RESTful_Elasticsearch_CURD 6]: /pro/os/crawler/QQ6N-BVYY-UMNY.jpg
[RESTful_Elasticsearch_CURD 7]: /pro/os/crawler/FYVZ-VIEJ-A6FN.jpg
[RESTful_Elasticsearch_CURD 8]: /pro/os/crawler/YAVU-J2ZE-QAFE.jpg
 *  **原文作者：** 编程技术栈
 *  **原文链接：** https://www.toutiao.com/item/6488447858893652494/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。