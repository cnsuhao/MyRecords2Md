---
title: 关于Access数据库乱码的问题
date: 2013-12-14 23:13:18
categories: "开发"
tags:
	- 数据库

---

``````````

``````````

因为机器装了太多东西,不想搞一个庞大的数据库,MySQL在之前我安装又卸载又安装卸载了几次之后,实在不想玩它了，于是想整一个占用资源少的数据库。

于是安装了Access. 微软的东西，没法说,实在是跟他当年说的 让编程更简单，玩了一下午，发现里面很多东西，例如设计方面的东西，真心值得去继续学习。

安装Access步骤:

1、本机是win7,之前安装了office2010 忘记什么版本了，后来里面缺少Access.因此首先需要下载access并按照它。

![INMB-6NAU-NIJJ.jpg][]

2、打开步骤：控制面板---管理工具----数据源(ODBC)---系统DSN---系统数据源（点击添加）

　　　![MJRR-VEMF-AVNU.jpg][]

到这步停下说几个问题： 我起初安装完access后创建了数据库，可在进行“选择（ｓ）．．”的时候，一直报路径非法什么的。

　　为了避免这个问题　直创建：

　　![ZQIR-MRQV-VVJJ.jpg][]![REZM-7JZV-AFBN.jpg][]

　　创建输入相应的值后，一直确定下去即可。

　３、打开ａｃｃｅｓｓ，手动创建一张Person表,表里添加四个字段:panme,,age,id,address. 其中pname原先命名为name。

![RVM2-2EVQ-FEVF.jpg][]

4、这些准备好之后,即需要测试数据库连接了：

``````````
package test.xue5i.db;

import java.sql.Connection;
import java.sql.DatabaseMetaData;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Properties;

import org.nobject.common.js.JSONObject;
import org.nobject.common.js.JSONUtils;

/**
 * @comments: 连接Access数据库
 * @author hanking(E-mail : IsaidIwillgoon@gmail.com)
 * @date:2013-12-14
 * @time:下午02:36:16
 * @version : 1.0
 */
public class AccessDb {

	public static void main(String[] args) {
		Connection con = null;
		Statement st = null;
		ResultSet rs = null;
		try {
			Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");// ODBC桥接池
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
		try {
			//解决了乱码读取的问题～
			Properties p = new Properties();
			p.setProperty("user", "");
			p.setProperty("password", "");
			p.setProperty("charSet", "gb2312");//gbk gb2312 
			con = DriverManager.getConnection("jdbc:odbc:XUE5I",p);
			st = con.createStatement();//st.executeUpdate("insert into Person(pname,age) values('米加奇',24)");
			rs = st.executeQuery("Select * FROM Person order by id");
			
			ResultSetMetaData rsmd =  rs.getMetaData();
			DatabaseMetaData dmd = con.getMetaData();
			String databaseName = dmd.getDatabaseProductName();//数据库产品名称
			int tableLength = rsmd.getColumnCount();
			String columnName = null,columnTypeName = null;
			List<Map<String,Object>> list = new ArrayList<Map<String,Object>>();
			Map<String,Object> map = null;
			Object obj = null;
			while (rs.next()) {
				map = new HashMap<String,Object>();
				for(int i=0;i<tableLength;i++){
					columnTypeName = rsmd.getColumnTypeName(i+1);//字段类型
					columnName = rsmd.getColumnName(i+1);//字段名称
					if("ACCESS".equals(databaseName)){//ACCESS数据库下
						if("COUNTER".equals(columnTypeName)){
							obj = rs.getInt(columnName);
						}else if("VARCHAR".equals(columnTypeName)){
							obj = rs.getString(columnName);
						}else if("INTEGER".equals(columnTypeName)){
							obj = rs.getInt(columnName);
						}else{
						}
					}
					map.put(columnName, obj);
				}
				list.add(map);
			}
			System.out.println(JSONUtils.toString(list));
			//System.out.println(list);
		}catch (SQLException el) {
			el.printStackTrace();
		}finally{
			try {
				if(rs!=null){rs.close();}
				if(st!=null){st.close();}
				if(con!=null){con.close();}
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}

}
``````````

因为懒的缘故,我是不想记这些字段的名字,所以就稍微根据jdbc提供的属性和方法之类的信息,将这些字段、类型进行获取并稍微进行了改变。这样针对一个SQL而言，只需知道SQL本身即可将它所需要查询的字段和值全部以JSON串的形式打印出来～～～。so....其中稍微又加了数据库判断，原因你懂的...

5、说一下乱码的问题:

终于到正题了. 我找了N久Access的编码设置或者是方式,均宣告无效,因为web项目本身是utf-8的,我不想为了它本身是中文的而全部更换为gbk的编码，所以网上的一些方式都摒弃了。那只好把解决办法放到字符转码的问题上。 那好我将数据查询的过程中 去查看rs的编码,发现是‘utf-8’ 于是就郁闷了，汉字出来都是"????" 为什么呢?

如果按access中文版网上说默认是gbk/bg2312之类的话，那通过字符转码应该没问题的。所以，很多说法均尝试以后，失败。。。。

后来，想起了conn 只好将希望寄托在连接创建的时候将字符编码设置好，让数据库知道. so....

``````````
Properties p = new Properties();
			p.setProperty("user", "");
			p.setProperty("password", "");
			p.setProperty("charSet", "gb2312");//gbk gb2312 
			con = DriverManager.getConnection("jdbc:odbc:XUE5I",p);
``````````

显得多么重要。刚刚去尝试了下：用sql插入一条有汉字的信息,并将字符设为utf-8,打印的结果是部分乱码，而存储到数据库里也是乱码。。。。。

![AI2Y-Q2QB-MJUF.jpg][]![RU6J-BZZQ-YZR3.jpg][]


![RU6J-BZZQ-YZR3.jpg][]


6、个人建议,乱码问题层出不穷,有时候为了减少这类的错误,尽量使用同一种编码,而为了今后可能使用第三方的东西,所以尽量都是有utf-8,因为gbk实在是伤不起...

　　　

　　资源下载页面:http://download.csdn.net/detail/xiaohan1990718/6715405


　

　　　






[INMB-6NAU-NIJJ.jpg]: /pro/os/crawler/INMB-6NAU-NIJJ.jpg
[MJRR-VEMF-AVNU.jpg]: /pro/os/crawler/MJRR-VEMF-AVNU.jpg
[ZQIR-MRQV-VVJJ.jpg]: /pro/os/crawler/ZQIR-MRQV-VVJJ.jpg
[REZM-7JZV-AFBN.jpg]: /pro/os/crawler/REZM-7JZV-AFBN.jpg
[RVM2-2EVQ-FEVF.jpg]: /pro/os/crawler/RVM2-2EVQ-FEVF.jpg
[AI2Y-Q2QB-MJUF.jpg]: /pro/os/crawler/AI2Y-Q2QB-MJUF.jpg
[RU6J-BZZQ-YZR3.jpg]: /pro/os/crawler/RU6J-BZZQ-YZR3.jpg