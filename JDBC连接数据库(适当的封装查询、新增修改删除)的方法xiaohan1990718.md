---
title: JDBC连接数据库(适当的封装查询、新增修改删除)的方法
date: 2012-11-12 23:19:47
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
					// String Str = rs.getString(i+1);
					// 做相应处理
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

//刚刚发现 修改测试让我忘记了~~sorry 纯手敲，错误难免。 以上例子受《Java编程百例》影响而写。代码稍后上传至资源。

//补充下：又看了下代码：当sql执行错误或者说异常后，事务需要回滚即 conn.rollback()，dome方法里忘记添加了。sorry...