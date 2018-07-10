---
title: JDBC连接数据库(适当的封装查询、新增修改删除)的方法改进
date: 2012-11-13 21:18:44
categories: "开发"
tags:
	- 数据库

---

``````````
package com.connection.ConnCommit;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.Types;
import java.util.HashMap;
import java.util.Map;

/**
 * 
 * @Detail :JDBC连接数据库
 * @Author: 韩庆
 * @E-mail: IsaidIwillgoon@gmail.com
 * @Date: 2012-11-12
 * @Time: 下午10:32:31
 * 
 */
public class JDBCConnCommit {

	private static String url = "jdbc:mysql://localhost:3306/nms";
	private static String user = "root";
	private static String password = "root";

	public Connection getConnection() {
		Connection conn = null;
		try {
			Class.forName("com.mysql.jdbc.Driver");
			conn = DriverManager.getConnection(url, user, password);

		} catch (ClassNotFoundException e) {
			e.printStackTrace();
			System.out.println("类未发现异常...");
		} catch (SQLException e) {
			e.printStackTrace();
			System.out.println("SQL异常....");
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("出现异常：" + e.getMessage());
		}
		return conn;
	}

	/**
	 * 执行查询操作
	 * 
	 * @param conn
	 *            数据库连接
	 * @param sql
	 *            需要执行的sql语句
	 * @throws Exception
	 */
	public void createSQLQuery(Connection conn, String sql) throws Exception {
		Statement stmt = null;
		ResultSet rs = null;
		Map map = new HashMap();//可获得执行查询后的map数据
		try {
			if (null == conn) {
				throw new Exception("连接为空！请检查数据库连接！");
			}
			if (null == sql || "".equals(sql)) {
				throw new Exception("sql语句为空！");
			}
			stmt = conn.createStatement();
			rs = stmt.executeQuery(sql);
			ResultSetMetaData rsmd = rs.getMetaData();
			// 用于得到字段个数
			int cCount = rsmd.getColumnCount();
			// 遍历每条数据 内部根据个人需求可做相应的处理
			while (rs.next()) {
				for (int i = 0; i < cCount; i++) {
					// 新知识-------改进部分
					String columnName = rsmd.getCatalogName((i+1)).toLowerCase();
					int sqlType = rsmd.getColumnType(i+1);
					Object sqlView = rs.getObject(columnName);
					switch (sqlType) {
					case Types.CHAR:
						map.put(columnName, sqlView.toString().trim());
						break;
					case Types.NUMERIC:
						if (null == sqlView) {
							sqlView = "";
						}
						map.put(columnName, sqlView.toString());
						break;
					default:
						map.put(columnName, sqlView != null ? sqlView.toString()
								: "");
						break;
					}
					
				}
			}

		} catch (SQLException e) {
			System.out.println(e.getMessage());
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (null != rs) {
				rs.close();
			}
			if (null != stmt) {
				stmt.close();
			}
			/*
			 * if (null != null) { conn.close();
			 * }//如果单纯的使用一次的话，则连接可以关闭createSQLExecute方法中也是
			 */
		}

	}

	/**
	 * 执行sql的添加、修改、删除操作
	 * 
	 * @param conn
	 * @param sql
	 */
	public void createSQLExecute(Connection conn, String sql) {
		// 有关conn和sql为空的问题，处理同上，省略。。。
		Statement stmt = null;
		try {
			stmt = conn.createStatement();
			int i = stmt.executeUpdate(sql);
			if (i > 0) {
				System.out.println("处理成功！处理条数为" + i);
			} else {
				System.out.println("处理有误！");
			}
		} catch (Exception e) {
			// 应该异常分类处理，这里省略
		} finally {
			try {
				if (null != stmt) {
					stmt.close();
				}
			} catch (SQLException e) {
				e.printStackTrace();
			}

		}
	}

	/**
	 * 测试样例
	 */
	public void exampleDome() {
		JDBCConnCommit jdbcCc = new JDBCConnCommit();
		Connection conn = jdbcCc.getConnection();// 得到连接
		try {
			conn.setAutoCommit(false);// 不自动提交事务
			String queryString = "";// 声明sql语句
			// 测试添加
			queryString = "insert into Table_Name(name,sex,age) values('hank','male',22)";
			jdbcCc.createSQLExecute(conn, queryString);
			// 测试查询
			queryString = "select * from Table_Name where name like '%韩%'";
			jdbcCc.createSQLQuery(conn, queryString);
			// 测试删除
			queryString = "delete from Table_Name where name='hank'";
			jdbcCc.createSQLExecute(conn, queryString);

			conn.commit();// 正常处理完后 提交事务
		} catch (SQLException e) {
			// ...
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				if (null != conn) {
					conn.close();// 关闭连接
				}
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}

	}

}
``````````


//就昨日封装做一点点改进，3Q 永泰~~~~~

仅上述查询方法而言：因为用到别处，所以要给个返回值的。

因而做进一步更改：

``````````
/**
	 * 执行查询操作
	 * 
	 * @param conn
	 *            数据库连接
	 * @param sql
	 *            需要执行的sql语句
	 * @throws Exception
	 */
	public Map createSQLQuery(Connection conn, String sql) throws Exception {
		Statement stmt = null;
		ResultSet rs = null;
		HashMap map = null;//可获得执行查询后的map数据
		try {
			if (null == conn) {
				throw new Exception("连接为空！请检查数据库连接！");
			}
			if (null == sql || "".equals(sql)) {
				throw new Exception("sql语句为空！");
			}
			stmt = conn.createStatement();
			rs = stmt.executeQuery(sql);
			ResultSetMetaData rsmd = rs.getMetaData();
			// 用于得到字段个数
			int cCount = rsmd.getColumnCount();
			map = new HashMap();
			// 遍历每条数据 内部根据个人需求可做相应的处理
			while (rs.next()) {
				for (int i = 0; i < cCount; i++) {
					// 新知识-------改进部分
					String columnName = rsmd.getCatalogName((i+1)).toLowerCase();
					int sqlType = rsmd.getColumnType(i+1);
					Object sqlView = rs.getObject(columnName);
					switch (sqlType) {
					case Types.CHAR:
						map.put(columnName, sqlView.toString().trim());
						break;
					case Types.NUMERIC:
						if (null == sqlView) {
							sqlView = "";
						}
						map.put(columnName, sqlView.toString());
						break;
					default:
						map.put(columnName, sqlView != null ? sqlView.toString()
								: "");
						break;
					}
					
				}
			}

		} catch (SQLException e) {
			System.out.println(e.getMessage());
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (null != rs) {
				rs.close();
			}
			if (null != stmt) {
				stmt.close();
			}
			/*
			 * if (null != null) { conn.close();
			 * }//如果单纯的使用一次的话，则连接可以关闭createSQLExecute方法中也是
			 */
			return map;
		}
	}
``````````


这样既可将查询出的所有数据以map格式返回给调用方法者。 以供做其他处理。

同理，新增、修改、删除均有返回值 如果执行正确则返回正整数，根据是否是正整数来判断是否执行成功。

//可拷贝，昨日已上传文件，做少许更改即可，故而今日不上传其资源了。




