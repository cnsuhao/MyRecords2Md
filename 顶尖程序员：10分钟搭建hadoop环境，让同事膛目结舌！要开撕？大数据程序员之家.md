---
title: 顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？
date: 2017-09-25 17:57:28
categories: "开发"
tags:
	- 程序员
	- Java
	- Cloudera
	- MySQL
	- 编程语言

---

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop]

> 这篇文章分享之前我还是要推荐下我自己的大数据**学习群：640193172**，不管你是小白还是大牛，小编我都挺欢迎，不定期分享干货，包括我自己整理的一份2017最新大数据资料和零基础入门教程，欢迎初学和进阶中的小伙伴

1、操作系统：

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 1]

2、ip与hostname配置关系

<table>
 <tbody>
  <tr>
   <td><p>ip</p></td>
   <td><p>hostname</p></td>
   <td><p>角色</p></td>
  </tr>
  <tr>
   <td><p>192.168.33.187</p></td>
   <td><p>cdh1</p></td>
   <td><p>主节点（master）</p></td>
  </tr>
  <tr>
   <td><p>192.168.33.191</p></td>
   <td><p>cdh2</p></td>
   <td><p>从节点（slave）</p></td>
  </tr>
  <tr>
   <td><p>192.168.33.192</p></td>
   <td><p>cdh3</p></td>
   <td><p>从节点（slave）</p></td>
  </tr>
 </tbody>
</table>

3、安装的工具：

3.1 jdk-8u144-linux-x64.rpm

下载路径：

http://download.oracle.com/otn/java/jdk/8u45-b14/jdk-8u144-linux-x64.rpm

下载jdk时需要登录

3.2 mysql和mysql驱动

mysql-5.7.19-1.el7.x86\_64.rpm-bundle.tar

下载路径：

https://dev.mysql.com/downloads/file/?id=471503

mysql-connector-java-5.1.42.tar.gz

下载路径：

https://dev.mysql.com/downloads/file/?id=470332

3.3 Cloudera-Manager-5.8.3和CDH-5.8.3

下载路径：

http://archive.cloudera.com/cm5/cm/5/cloudera-manager-centos7-cm5.8.3\_x86\_64.tar.gz

CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel.sha1、CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel、manifest.json

下载路径：

http://archive.cloudera.com/cdh5/parcels/5.8.3/CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel.sha1

http://archive.cloudera.com/cdh5/parcels/5.8.3/CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel

http://archive.cloudera.com/cdh5/parcels/5.8.3/manifest.json

下载manifest.json可能没有内容显示，多尝试几次

4、修改hostname

所有节点都要修改hostname，修改hostname是为了方便使用。在CentOS7系统中修改hostname可以使用hostnamectl命令，使用此命令不需要在特定的文件中执行也不需要重启服务器。在所有节点的root权限下执行命令：

hostnamectl set-hostname cdh1

hostnamectl set-hostname cdh2

hostnamectl set-hostname cdh3

可以使用hostnamectl或者hostnamectl status命令查看hostname的修改情况。

5、hostname与ip地址映射

把hostname与ip地址进行映射对ssh无密码登录、cdh配置和scp命令使用等都有益处。在所有节点root权限下执行：

vi /etc/hosts

把/etc/hosts中默认的内容删除掉，添加以下内容：

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 2]

6、关闭selinux和防火墙

关闭所有节点的selinux和防火墙。在root权限下执行命令：vi /etc/sysconfig/selinux

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 3]在root权限下执行命令关闭防火墙：

systemctl stop firewalld

systemctl disable firewalld

使用命令查看防火墙状态：

systemctl status firewalld

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 4]

7、SSH无密码登录

集群是完全在root权限下，因此需要配置root用户的ssh无密码登录，ssh无密码登录是为了hadoop集群中各个节点之间的无密码连通。

在所有节点的root用户下执行命令：

ssh-keygen -t rsa

几次enter后在/root/.ssh/目录下有id\_rsa、id\_rsa.pub和known\_hosts三个文件。

在cdh2执行：

cp /root/.ssh/id\_rsa.pub id\_rsa.pub001

scp -r /root/.ssh/id\_rsa.pub001 zysdslavemaster000:/root/.ssh/

在此执行scp命令会用到root的密码

在cdh3中执行：

cp /root/.ssh/id\_rsa.pub id\_rsa.pub002

scp -r /root/.ssh/id\_rsa.pub002 zysdslavemaster000:/root/.ssh/

在此执行scp命令会用到root的密码

以上两个执行是为了对id\_rsa.pub备份，防止id\_rsa.pub破坏

在cdh1中的/root/.ssh/路径中有id\_rsa.pub、id\_rsa.pub001、id\_rsa.pub002三个要使用的文件，

执行命令：

cat id\_rsa.pub >> authorized\_keys

cat id\_rsa.pub001 >> authorized\_keys

cat id\_rsa.pub002 >> authorized\_keys

把生成的authorized\_keys复制到cdh2、cdh3中的/root/.ssh/中，执行命令：

scp -r /root/.ssh/authorized\_keys cdh2:/root/.ssh/

scp -r /root/.ssh/authorized\_keys cdh3:/root/.ssh/

在此scp命令需要使用root的密码

如果是在普通用户下要对所有节点中的authorized\_keys设置权限：

chmod 700 /用户名称/.ssh/

chmod 600 authorized\_keys

至此ssh无密码登录完成。

8、jdk安装

在此使用的jdk版本为：jdk-8u144-linux-x64.rpm

使用的jdk我安装在root权限下，执行命令：

rpm -ivh jdk-8u144-linux-x64.rpm

完成，jdk默认安装在/usr/java/jdk1.8.0\_45目录下

配置环境变量：

vi /etc/profile

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 5]

使环境变量生效，命令：source /etc/profile

9、mysql

CDH需要使用mysql数据库，我的数据库配置在59.110.60.203服务器上，mysql的配置不再阐述。如果本身集群的软硬件条件欠佳，没有必要非得把mysql与cloudera-manager安装在同一台服务器上。

所有的节点的/usr/share/java/目录下都要有java的驱动jar包

把mysql的驱动放在/usr/share/java/路径下，如果没有/usr/share/java路径则手动建，使用cp顺便把mysql驱动名称修改为：mysql-connector-java.jar。执行命令：

cp mysql-connector-java-5.1.42-bin.jar /usr/share/java/mysql-connector-java.jar

10、安装Cloudera-Manager

10.1 在所有节点root目录下执行。

mkdir /opt/cloudera-manager

解压cloudera-manager，在所有节点的root权限下执行：

tar -axvf cloudera-manager-centos7-cm5.8.3\_x86\_64.tar.gz -C /opt/cloudera-manager

10.2 在所有节点root用户下创建cloudera-scm用户

useradd -r -d /opt/cloudera-manager/cm-5.8.3/run/cloudera-scm-server -M -c "Cloudera SCM User" cloudera-scm

10.3 在cdh2和cdh3的节点（从节点）上配置agent，把server\_host修改成master的hostname（ip亦可），其中master节点是否修改均可。

vim /opt/cloudera-manager/cm-5.8.3/etc/cloudera-scm-agent/config.ini

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 6]

10.4在（master）主节点中创建parcel-repo仓库

mkdir -p /opt/cloudera/parcel-repo

chown cloudera-scm:cloudera-scm /opt/cloudera/parcel-repo

把CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel.sha1中的1符号去掉。

mv CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel.sha1 CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel.sha

把parcel、parcel.sha和manifest.json放到/opt/cloudera/parcel-repo中

cp CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel CDH-5.8.3-1.cdh5.8.3.p0.2-el7.parcel.sha manifest.json /opt/cloudera/parcel-repo

Clouder-Manager将CDHs从主节点的/opt/cloudera/parcel-repo目录中抽取出来，分发解压激活到各个节点的/opt/cloudera/parcels目录中。

10.5 因为我的mysql创建在59.110.60.203服务器中，要注意相应的ip和用户名，因此在主节点（master）初始脚本配置数据库scm\_prepare\_database.sh：

/opt/cloudera-manager/cm-5.8.3/share/cmf/schema/scm\_prepare\_database.sh mysql -h59.110.60.203 -uroot -pzysdadmin123 --scm-host 192.168.33.187 scmdbn scmdbu scmdbp

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 7]

如果mysql和cloudera-manager安装在同一台服务器上，以mysql和cloudera-manager都放在cdh1服务器上为例，则：

/opt/cloudera-manager/cm-5.8.3/share/cmf/schema/scm\_prepare\_database.sh mysql -hcdh1 -uroot -pzysdadmin123 --scm-host cdh1 scmdbn scmdbu scmdbp

其中：mysql：数据库用的是mysql，如果安装过程中用的oracle，那么该参数就应该改为oracle。

\-hhadoop1：数据库建立在hadoop1主机上面。也就是主节点上面。

\-uroot：root身份运行mysql。-pzysdadmin123：mysql的root密码。

\--scm-host cdh1：CMS的主机，一般是和mysql安装的主机是在同一个主机上。

最后三个参数是：数据库名，数据库用户名，数据库密码。

11、各个节点启动agent服务

/opt/cloudera-manager/cm-5.8.3/etc/init.d/cloudera-scm-agent start

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 8]

在主节点（master）中启动server

/opt/cloudera-manager/cm-5.8.3/etc/init.d/cloudera-scm-server start

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 9]

等待几分钟后在任一浏览器上查看

http://192.168.33.187:7180/cmf/login

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 10]

出现登录界面说明配置成功，使用默认的用户名和密码（用户名和密码都是admin）登录，使用界面式的安装CDH-5.8.3。

![顶尖程序员：10分钟搭建hadoop环境，让同事膛目结舌！要开撕？][10_hadoop 11]

注意数据库主机的名称

如果由于某种原因，可能要重新安装CDH，要保证整个集群中各个服务器没有CDH的残留，具体要残留的CDH文件的路径还需要进一步的排查

> 这篇文章分享之后我还是要推荐下我自己的大数据**学习群：640193172**，不管你是小白还是大牛，小编我都挺欢迎，不定期分享干货，包括我自己整理的一份2017最新大数据资料和零基础入门教程，欢迎初学和进阶中的小伙伴


[10_hadoop]: /pro/os/crawler/YVYF-M2N3-IAB3.jpg
[10_hadoop 1]: /pro/os/crawler/UQN7-ZUF7-BVE2.jpg
[10_hadoop 2]: /pro/os/crawler/V2EN-MNAV-NENA.jpg
[10_hadoop 3]: /pro/os/crawler/3EF6-FEJF-UZMY.jpg
[10_hadoop 4]: /pro/os/crawler/YJMA-YVEV-3EI2.jpg
[10_hadoop 5]: /pro/os/crawler/FMEY-Z3MQ-RUNN.jpg
[10_hadoop 6]: /pro/os/crawler/32U6-NRNI-U7VE.jpg
[10_hadoop 7]: /pro/os/crawler/RJNM-JBZZ-ZEUA.jpg
[10_hadoop 8]: /pro/os/crawler/2MRE-NREM-Z2A3.jpg
[10_hadoop 9]: /pro/os/crawler/RMBI-JAAF-6F3Y.jpg
[10_hadoop 10]: /pro/os/crawler/UZRJ-7ZE6-FJQ2.jpg
[10_hadoop 11]: /pro/os/crawler/RRIA-QN7V-YZVQ.jpg
 *  **原文作者：** 大数据程序员之家
 *  **原文链接：** https://www.toutiao.com/item/6469546183655162381/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。