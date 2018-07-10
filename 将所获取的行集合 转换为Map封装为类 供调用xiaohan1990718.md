---
title: 将所获取的行集合 转换为Map封装为类 供调用
date: 2012-11-13 21:26:33
categories: "开发"
tags:
	- 技术
	- javascript

---

``````````
package com.connection.ConnCommit;

import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Types;
import java.util.HashMap;

/**
 * 
 * @Detail : 将所获取的行集合 转换为Map
 * @Author: 韩庆
 * @E-mail: IsaidIwillgoon@gmail.com
 * @Date: 2012-11-13
 * @Time: 下午8:44:33
 * 
 */
public class RawMapper {

	public ResultSetMetaData rsmd = null;
	public int rawCount = 0;

	public Object mapRow(ResultSet rs, int rowNum) {
		HashMap map = new HashMap();
		try {
			// 如果rsmd为空
			if (null == rsmd) {
				rsmd = rs.getMetaData();
				rawCount = rsmd.getColumnCount();
			}
			for (int i = 0; i < rawCount; i++) {
				String columnName = rsmd.getCatalogName((i + 1)).toLowerCase();// 得到列名称
																				// 并转换小写
				int sqlType = rsmd.getColumnType(i + 1);
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
		} catch (SQLException sqlE) {
			// ...
		} catch (Exception e) {
			// ...
		}
		return map;
	}
}
``````````

将其封装起来 以便之后调用。//此形式可以用于 this. getJdbcTemplate.query(sql,new mapper()\{

//内部实现

\} );

是SSH里提供的另一种查询方式。//3Q 永泰~~