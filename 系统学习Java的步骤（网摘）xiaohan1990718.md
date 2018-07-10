---
title: 系统学习Java的步骤（网摘）
date: 2012-11-07 00:07:41
categories: "开发"
tags:
	- Java

---

1. Java语言基础
谈到Java语言基础学习的书籍，大家肯定会推荐Bruce Eckel的《Thinking in Java》。它是一本写的相当深刻的技术书籍，Java语言基础部分基本没有其它任何一本书可以超越它。该书的作者Bruce Eckel在网络上被称为天才的投机者，作者的《Thinking in C++》在1995年曾获SoftwareDevelopment Jolt Award最佳书籍大奖，《Thinking in Java》被评为1999年Java World“最爱读者欢迎图书”，并且赢得了编辑首选图书奖。作者从1986年至今，已经发表了超过150篇计算机技术文章，出版了6本书（其中4本是关于C++的），并且在全世界做了数百次演讲。他是《Thinking in Java》、《Thinking in C++》、《C++ Inside & Out》《Using C++》和《Thinking in Patterns》的作者，同时还是《Black Belt C++》文集的编辑。他的书被读者称为“最好的Java参考书……绝对让人震惊”；“购买Java参考书最明智的选择”；“我见过的最棒的编程指南”。作者的非凡才华，极其跨越语言的能力，使作者被选为Java发展10年间与Java关系最密切的10个人物之一。
《Thinking in Java》讲述了Java语言的方方面面，很多Java语言的老手都评价“这是一本将Java语言讲得相当丑陋的书”。该书谈及了java语言的很多细节，每一个方面都是相当深刻的。通过本书你可以看到“丑陋的”java语言。
网络上关于java语言讲解的视频很多很多，其中不凡有垃圾。《翁恺—JAVA语言》可能是你学习java语言基础的唯一选择，该讲座基本按照《Thinking in Java》这本书讲解，其中不凡有翁老师的很多有意思的笑话。我很幸运学习就是从此视频开始的。内容包括30讲，我总共看了3遍。
不过，对于初学者我不太推荐使用《Thinking in Java》，我比较推荐Prentice Hall PTR 的《Core Java 2》国内称为《Java 2 核心技术》，目前是第七版。网络上大家都可以下载到电子版。Oreilly的《Java in a nutshell》也是一个不错的选择。读完以上两本后，你可以看看翁恺老师的视频，接着可以研究《Thinking in Java》了。
2. Java数据结构
市面上关于Java数据结构的书本身就很少很少。大致有APress 的《Java Collections》，Jones 和Bartlett 的《Data Structures in Java》、《Object-oriented Data Structures Using Java》以及Prentice Hall 出版的《Data Structures and Algorithms in Java》 (Dec 19, 2005)还有一本就是《Data Structures And Algorithms With Object-oriented Design Patterns In Java》。很幸运我的第一本英文书就是APress 的《Java Collections》（本书在国内可能根本就没有中文版――只能下载英文版了），很不错，讲得很有条例、很简单，是一本完完全全Java Collections API介绍的书籍，其中不凡有扩展API的例子。这是我推荐你学习java数据结构的唯一一本好书。其它的Jones 和Bartlett的那两本国内好像有一本中文版，想看你也可以看看。
在学习完API后，你可以看看java.util包中对应的类了。不过只有在学习过设计模式后你才有可能完全理解整个Java Collections Framework。Java Collections Framework使用了很多著名的设计模式如：迭代器（Iterator）模式，工厂方法模式、装饰器模式、适配器模式等等。通过研究 java.util包中数据结构的源代码，你可以知道臭名昭著的Properties类的设计了，同时可能基本具备设计简单的数据结构的能力了。
所谓学习无止境，学习完Sun提供了Java Collections Framework后，你可以研究Apche的另一个Java Collections Framework，很有意思哦。互为补充的两个Framework。
在大家学习、研究Java Collections之前，我提示一下Java Collections主要包括以下三部分：接口（Interface）、实现（Implemention）和算法（Algorithm）。
1. 接口主要有List、Set、Queue和 Map。List 、Se t和Queue是 Collection接口的子接口。
2. 实现主要是实现这些接口的具体类。如实现List接口的ArrayList、LinkedList、Stack和Vector；实现Set接口的 HashSet、TreeSet 和LinkedHashSet；实现Queue接口的PriorityQueue、SynchronousQueue等等；实现Map接口的 HashMap、TreeMap、Hashtable、Properties、WeakHashMap等等。
3. 算法主要是由Arrays类和Collections类提供的，它是整个Java Collection Framework算法的核心。支持各种类型的排序，查找等常用操作。
Java Collections中包含两个版本的数据结构，主要是原先的支持同步的数据结构和后来不支持同步的数据结构。
Java Collection Framework在使用Comparator和Comparable接口支持排序。同时提供新旧两个版本的迭代器Iterator和Enumeraton，以及它们如何转换等等。
在java.util包中的Obserable接口和Observer类是考察者模式的核心。
……
3. Java IO
市面上关于IO的书籍也仅仅只有Oreilly出版社的两本，都是Elliotte Rusty Harold的著作。两本书的风格基本一致，推荐阅读是第一版的《Jvava I/O》，讲得比较浅显，内容相对比较集中，实例也很多。第二版今年5月国外才出版，很有幸我在网络上下载了第二版，讲得极其详细――726页的大块头（我化了两个星期），这次将NIO和IO和在一起，还包括J2ME部分的，不过串口、并口通信部分好像类库支持不够，自己不能实际操作。
与第一版的《Jvava I/O》一起的Oreilly还有一本《Jvava NIO》，也是很不错的哦。
大家在依次阅读完《Jvava I/O》以及《Jvava NIO》后，可以研究java.io包中的源代码了。在大家研究源代码前我给点提示：
Java的io包主要包括：
1. 两种流：字节流（byte Stream）和字符流（character stream），这两种流不存在所谓的谁代替谁、谁比谁高级之说，它们互为补充，只是侧重点不同而已。
2. 两种对称：1.字节流、字符流的对称；2.输入、输出的对称。
3. 一个桥梁：将字节流转变为字符流的InputStreamReader和OutputStreamWriter。
其中必须注意：
1. PipedInputStream和PipedOutputStrem是两个比较有趣的类。
2. 支持Buffered的流是我们经常使用的类。
3. 装饰器（Decorator）模式在java最著名的应用就是用于io的设计。仔细研究各个Filter流与具体流的关系，多看设计模式的书籍。相信你会有所所获。
4. 学习好io包，是研究net包，rmi包……的基础哦！
4 . Java数据库
数据库的书籍太多太多了，也是太烂太烂了！这方面的书我基本都研究过，推荐的你就看看Apress的《JDBC Recipes A Problem Solution Approach 》很不错，国外2005年底才出版，（国内好像没有中文版，不过出了中文版也不一定值得看――国内经常将国外的书翻译得一塌糊涂、不堪入目）不过我们真的很幸运，网络上有电子版的。值得一看。推荐我看的第一本比较满意的――Wiley出版的《Java Database Bible》，讲得很不错！Sun公司自己的关于JDBC API介绍的那一本《JDBC API Tutorial andRefernece》也不错。我第二本JDBC的就是研究的这套API。
不过目前这些书都是一些相对比较浮浅的API应用的书籍。有机会我会给大家带来介绍JDBC API以及JDBC实现内部细节的书！我尽快努力，同时希望得到大家的支持！
顺便给学习JDBC的朋友一点提示：
JDBC的学习和使用主要是这套API，其使用过程也是极其简单，下面是使用JDBC的一般流程：
1. 加载某个数据库的驱动（Driver类），通常使用Class.forName(“驱动的类名“);
2. 连接数据库――
Connection con = DriverManager.getConnection(url,username,password)；
3. 得到会话――Statement stmt = con.createStatement();
4. 执行操作――Result rs = stmt.executeQuery(“SQL查询语句”)；
5. 处理结果――
while(rs.next())\{
String col1 = rs.getString(1);
……
\}
简单吧！整个JDBC中可以变化的一般是：
1. 可以由Connection对象创建Statement、PreparedStatement和CallableStatement创建三种类型的Statement。
2. 可以创建多种类型的ResultSet：支持单向移动和个自由移动；可更新的和不可更新的；支持不同等级的交易的…..
3. 数据输入的批处理。
4. 结果集中特殊类型（Blob、Clob、Arrary和Ref、Struct）列的操作。
5. 这些特殊类型的录入数据库。
6. javax.sql包中特殊结果集（CachedRowSet、JdbcRowSet、WebRowSet）的操作。
7. 其它的就是一个DataSource了，也很简单！一个J2EE中的被管理对象
简单吧！相信大家很快就会征服JDBC。
5. Java 网络编程
网络编程――一个神秘的、充满挑战的方向。不过在谈Java网络编程之前首先感谢Sun公司的开发人员，因为它们天才的设想，充满智慧的架构，使广大java程序员学习java网络编程变得异常简单。
Java网络编程方面的书，我推荐O'Reilly的《Java Network Programming》，目前已经第三版了，以前的版本市面上肯定有！网络上早有第三版的电子版，国外2004年出版，706页哦！讲得很全，比较深入，太深入的可能由于Sun有些东西没有完全公开，所以也就不好讲了，有兴趣的可以下载看看！第二本还是O'Reilly 1998年出版的《Java distributed computing 》，基础部分写得比较详细，后面的实例还是值得研究的。
在大家阅读这些书之前，给大家一点提示：
java网络编程其实相对比较简单，入门也很快很快。java网络编程主要包括两个部分：1.Socket；2.URL部分。不过第二部分也完全建立在第一部分的基础上。
1. Socket包括客户端的Socket和服务器端的ServerSocket。还有就是DatagramSocket和DatagramPacket，它对应于UDP通信协议。 总之，Socket部分是建立其它高级协议的基础。
2. URL类是一个网络资源定位器，通常和具体的网络协议如HTTP，FTP，Telnet……相关。通过该类可以连接网络上的资源，通过其 openStream可以以io包中的流（InputStream）的形式读取网络资源；通过其OpenConnection方法，可以打开一个连接，在此连接上可以不仅可以完成读的操作，还可以完成写的操作。
Java的网络编程大体包括以上两部分。网络编程和IO以及多线程部分非常密切，在学习此部分前大家一定对这两部分了解比较透彻。
学习了以上部分你可以研究java.net包中的与此相关的源代码了！研究所有的源代码还为时尚早。在整个net包中包含： ContentHandlerFactory、URLStreamHandlerFactory、URLStreamHandler、 URLClassLoader等辅助类，它们构成了java.net网络编程的框架，通过研究其源代码，你不仅可以快速理解java.net包，还可以为以后扩展该包打下基础，甚至可以将此思维方式运用到自己的项目中。
到此为止你对java.net包应该才了解60％，还有一部分你可以使用JDecompiler之类的反编译软件打开你JDK安装目录下\\jdkxxx\\ jre\\lib目录中的rt.jar，用WinRAR之类的软件打开它的sun.net包，反编译所有的文件，它是URL类工作的细节。当研究完该 sun.net包，你就会对整个网络编程很熟悉很熟悉了。
一切看起来我们已经对网络编程很精通了。其实不然，刚刚开始而已，要想深入，请继续吧！网络上很多优秀的网络编程库甚至软件可以为我们“添加功力”。如 Apache的HttpCore和HTTPConnection 是两个和HTTP协议相关库；JGroups是研究分布式通信、群组通信的必读库；接着我们可以研究P2P的软件包，如Sun公司的JXTA，它可能是 java平台点对点通信未来的标准哦！接着你可以研究成熟得不得了，使用极其广泛得P2P软件Azureus！www.sourceforge.net可以下载到！
千里之行始于足下！Just do it ！（目前我也只研究了net包，其它的会在不久的将来继续深入。Sun公司因为某些原因没有公开net的其它实现细节，在其允许将其源代码以文字的形式加以研究，以及允许将其没有公开的实现写入书中时，我很希望能出一本java网络编程的书籍，以飧广大读者！！）
6. Servlet和JSP
Servlet、JSP的书也是满地都是！值得推荐的也仅仅两三本。实推Addison Wiley的《Servlets and JavaServer pages ：The J2EE Technology Web Tier》，又是一本很厚的哦！国外2003年出版、784页，讲得比较全，例子也很多，特别是第八章Filter，举了几个不错的例子。其它所有我看到的关于Servlet和JSP的书都没有如此深入的！（可能有我没有看到而已）。O’reilly的《Java Servlet Programming》和《Java Server Pages》相对比较好懂一些，可以读读！
在大家学习Servlet和Jsp之前我还是要提醒一下：
本质上说Servlet就是一个实现Servlet接口的、部署于服务器端的服务器端的程序罢了！它可以象写其它任何java应用程序一样编写，它可以操作数据库、可以操作本地文件、可以连接本地EJB……编写Servlet程序的一般流程为：
1. 继承一个HttpServlet类；
2. 覆盖其doGet、doPost方法；
3. 在覆盖方法的内部操作方法参数HttpServletRequest和HttpServletResponse。
4. 读取请求利用HttpServletRequest。利用HttpServletRequest你可以操作Http协议的协议头、可以得到请求的操作方法、可以得到请求的路径、可以得到请求的字符串、以及和请求客户相关的信息，更主要的你可以得到Cookie和HttpSession这两个对象。
5. 利用Cookie你可以操作“甜心”对象或者将其写入HttpServletResponse中。
6. 向客户输出信息可以使用HttpServletResponse。使用HttpServletResponse可以写入各种类型的协议头、可以增加Cookie、可以重定向其它URL、可以向客户发送Http协议的状态码。
7. 利用HttpSession在会话内完成你想实现的任何功能。
同时Servlet还提供了一些事件和事件监听器（简单的观察者模式而已）。还有就是过滤器（Filter）和包装器（ServletRequestWrapper、ServletResponseWrapper）――简单的流的使用和装饰器模式的使用。
学习Sevlet、JSP必然要部署到服务器中，记住通常文件部署的步骤和参数的设置以及在程序中如何使用就可以了。
完全理解Servlet后，学习jsp相对比较容易了！Jsp完全建立在Servlet的基础上，它是为了迎合那些喜欢在Html文档中嵌入脚本（如：PHP之类的网页编程语言）的程序员的需要罢了！学起来也相当的容易！
一切看起来似乎那么的风平浪静，简单好学！简单的表象背后有其复杂的机理。要想对Servlet和Jsp彻底研究，你得研究Tomcat等开源软件的具体实现。它无非就是一个服务器，在客户利用网页通过HTTP协议向服务器发送请求后，服务器将此HTTP请求转化为相应的 HttpServletRequest对象，调用你编写的Servlet罢了，在你的Servlet中你肯定操作了此 HttpServletRequest了吧，同时操作了HttpServletResponse了吧，服务器就将此 HttpServletResponse按照HTTP协议的要求利用HTTP协议发送给你的浏览器了！在服务器端的Jsp网页在被客户请求后， Tomcat会利用编译软件，使用javax.servlet.jsp包中的模板，编译此jsp文件，编译后就是一个Servlet！以后的操作和 Servlet完全一样哦！
在Servlet和Jsp的基础上出现了，所谓的高级技术：JSTL，Struts……无非就是一些标签和MVC模式的使用。
继续前进吧！胜利就在前方！！
7. 多线程
一个看起来很神秘，却很容易上手、很难精通的方向！
我推荐两本我感觉很好的书籍。首先是我第一本能上手看的这方面的书，Sams 1998年出版的《Java Thread Programming》，写得暴好，很容易读懂，我有空还时常看当时的笔记！要知道怎么好你自己看吧！第二本OReilly三次出版的《Java Threads》，最新是2004版，国内好像有中文版，推荐你还是看英文版的吧！书中谈到了与多线程相关的N个方向，如IO、Swing、 Collection等等。
给大家一点提示吧！java类库中与多线程相关的类不是很多，主要有：Thread、ThreadGroup以及ThreadLocal和 InheritableThreadLocal四个类和一个Runnable接口；关键字synchronize、volatile ；以及Object对象的wait、notify、notifyAll方法！
1 Thread是多线程的核心类，提供了一系列创建和操作多线程的方法。
2 ThreadGroup是一个管理Thread的工具类。
3 ThreadLocal和InheritableThreadLocal为Thread提供了一个类似保险箱功能的存储线程对象的类！
4 Runnable不用说了吧！
5 synchronize是同步方法和同步块的核心哦！多个线程调用此方法时，只有一个线程可以使用此方法，其它方法阻塞，从而保证被操作对象内部状态完整性。某个线程调用带有synchronize的方法或块时会得到该对象的对象锁，完成块中的操作后释放此对象锁，从而其它对象可以继续操作。
6 wait、notify、notifyAll提供了有效的等待/通知机制。Java语言中每一个对象都有一个休息室，任何线程在其操作的对象的状态不满足的情况下，在该对象的休息室中休息，释放对象锁；当其它线程操作该对象后，唤醒休息室中的线程，它们再检查条件，当条件满足后，执行相应的操作。
多线程大致就这么多基础的！简单吗！这对于一个真正的程序员应该是不够的，真正对多线程要有所掌握，请您研究java.util.concurrent包吧！大师Doug Lea的作品，原先是一个开源的一致性编程的库，后来被Sun公司并入java类库。作者的网站上也有另外一个版本的该类库！值得研究的好东西! Hibernation、OpenJMS等开源软件都使用了此包！
8. 设计模式
谈到设计模式很多人多会推荐GOF的那本，该书在Amzon上是五星级的推荐书籍。不过对于学习java没多久的、特别是java初学者，我很不推荐这本书。主要是该书的例子基本都是C++的，很多细节没有讲述得足够清楚。
我给大家推荐的第一本是阎宏博士的《Java 与模式》，它是第一本中国人自己写的关于设计模式的书籍，写的比较有趣，融合了很多中华民族的文化和观念，例子、类图都比较多，且相对简单！非常不错的入门书籍――又是大块头哦！
其次我推荐Wiley出版社出版的《Pattern In Java》一套三本，我才看了第一本，好像第二本不怎么样，第三本还不错！
第三本是中文翻译版的关于多线程模式的（很难得的中文翻译版）中国铁道出版社2003年出版的《Java多线程设计模式》，将多线程模式讲得非常浅显，配有大量的图例，每章都有习题，最后有答案！我研究多线程模式就是由它开始的！
第四本，今年出版的Head First系列的《Head First Design Pattern》，秉承Head First系列图书的优点，大量的类图、丰富的实例、有趣的注解，值得购买！
其次在J2EE方向你可以研究阅读Addison Wesley 2002年出版的《Patterns of Enterprise Application Architecture》，众多大腕的作品，讲企业消息集成的！Sun提供的《J2EE PATTERNS SL500》也很好！晚了推荐那一本Amzon 4星半的《Holub on patterns》，大师的作品，提供了，很值得研究的例子，不过对上面四本不是很熟悉的读者，最好不要读它！可能会让你比较累！
我学习设计模式经过一段很曲折的路线，前前后后大约看了20本，阎宏博士的《Java 与模式》我看了4遍，还排除我第一次基本没看懂的看！记得研一时老师给我们讲了GOF的那本，作为选修课，我和它们计算机系的硕士、博士们一起，到最后一个班40－50个人，不超过3个人明白，我也没有明白任何一点（基础差吧――主要我对C++语言一点都不了解），凭我不伏输的性格，我认为我对java语言理解还可以，我就借了《Java 与模式》，结果还是基本没看懂。很有幸的是读研三时，听过了上交大饶若楠老师关于Java OOP语言的讲座，我懂了组合书籍模式等三种设计模式后，对其它模式有了强烈的兴趣和要征服它的愿望！工作后我买的第一本就是《Java 与模式》，第一遍花了2个月研究了这个1000多页的大块头，后来第三遍15天左右就可以搞定，笔记记了一大本！从此一发不可收拾。
选对书、埋头研究。相信很快就会入门的！
学习Java语言8个简单的部分，这只是我们研究Java语言的开始！这些都懂了充其量一个java程序员而已，后面的路很长很长！我们可以继续研究数据库实现的源代码、Servlet服务器的源代码、RMI、EJB、JNDI、面向方面编程、重构、ANT工具、Eclipse工具、Spring工具、 JBoss、JOnAS、Apache Geronimo等J2EE服务器！研究了这些你可能会成为一个出色的J2EE Architecture！你可以继续研究剖析器、编译器、JNODE（java写的操作系统）……