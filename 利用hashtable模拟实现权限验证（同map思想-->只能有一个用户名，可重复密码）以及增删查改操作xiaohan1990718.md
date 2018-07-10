---
title: 利用hashtable模拟实现权限验证（同map思想-->只能有一个用户名，可重复密码）以及增删查改操作
date: 2012-11-28 09:20:31
categories: "开发"
tags:
	- 技术
	- javascript

---

``````````
package mapPractice;

import java.util.Enumeration;
import java.util.Hashtable;

/**
 * 
 * @Detail :利用hashtable实现权限验证（同map思想-->只能有一个用户名，可重复密码）以及增删查改操作
 * @Author: 韩庆（QQ:haw_king@foxmail.com）
 * @E-mail: IsaidIwillgoon@gmail.com
 * @Date: 2012-11-27
 * @Time: 下午8:58:51
 * 
 */
public class HashForRole {

	private static Hashtable<String, String> rightTable = new Hashtable<String, String>();

	/**
	 * 初始化
	 */
	public void init() {
		String[] nameList = { "hank", "hanq", "laoH", "laogao" };
		String[] passwordList = { "123456", "1314666", "password",
				"admin_password" };
		for (int i = 0; i < nameList.length; i++) {
			rightTable.put(nameList[i], passwordList[i]);
		}
		System.out.println("初始化了rightTable.");
	}

	/**
	 * 插入/新增操作
	 * 
	 * @param name
	 * @param password
	 */
	public void insert(String name, String password) {
		// 如果用户名不存在
		if (rightTable.get(name) == null && null != password) {
			rightTable.put(name, password);
			System.out.println("做了插入操作,将用户名:" + name + ",密码:" + password
					+ "插入.");
		} else {
			if (null == password) {
				System.out.println("密码不可为空!");
			} else {
				System.out.println("用户名" + name + ",已存在,请更换名称!");
			}
		}
	}

	/**
	 * 更新用户
	 * 
	 * @param name
	 *            用户名
	 * @param password
	 *            密码
	 */
	public void update(String name, String password) {
		// boolean flag = rightTable.containsKey(name);
		if (rightTable.get(name) == null) {
			System.out.println("无此用户!修改失败");
		} else {
			String pd = rightTable.get(name);
			rightTable.put(name, password);
			System.out.println("修改了用户:" + name + ",原密码为:" + pd + ",现密码变更为："
					+ password);
		}
	}

	/**
	 * 删除用户名
	 * 
	 * @param name
	 *            用户名
	 */
	public void delete(String name) {
		// 如果该用户存在
		if (rightTable.containsKey(name)) {
			rightTable.remove(name);
			System.out.println("删除了用户:" + name);
		} else {
			System.out.println("该用户已被删除,或不存在!");
		}
	}

	/**
	 * 得到用户权限
	 * 
	 * @param name
	 *            用户名
	 * @param password
	 *            密码
	 * @return 用户密码
	 */
	public Boolean getRight(String name, String password) {
		if (password == rightTable.get(name)) {
			System.out.println("得到用户权限为-->" + name + ":" + password);
			return true;
		} else {
			System.out.println("用户名或密码错误");
			return false;
		}
	}

	/**
	 * 遍历查询rightTable所有元素
	 */
	public void query() {
		Enumeration<String> allKey = rightTable.keys();
		System.out.println("rightTable内的数据为:");
		while (allKey.hasMoreElements()) {
			String key = allKey.nextElement().toString();
			System.out.print(key + ":" + rightTable.get(key) + "  ");
		}
		System.out.println();// 换行
	}

	/**
	 * 仅用于测试 是否包含name
	 * 
	 * @param name
	 * @return
	 */
	public boolean find(String name) {
		return rightTable.containsKey(name);// 包含name返回true.
	}

	public static void main(String[] args) {
		// 测试程序
		HashForRole hfr = new HashForRole();
		hfr.init();// 初始化
		hfr.query();// 遍历查看初始化后表内数据，与执行以下操作后遍历对照。
		Boolean rolePassword = hfr.getRight("hank", "123456");
		if (rolePassword) {
			hfr.insert("newRole", "newPassword");// 模拟添加
		}
		System.out.println("----test one-----");
		Boolean rolePassword2 = hfr.getRight("newRole", "newPassword");
		if (rolePassword2) {
			hfr.update("newRole", "oldPassword");// 模拟修改
		}
		System.out.println("----test tow----");
		Boolean rolePassword3 = hfr.getRight("newRole", "oldPassword");
		System.out.println("----test three----");
		if (rolePassword3) {
			hfr.delete("hank");// 模拟删除
			hfr.query();// 遍历查询结果集
		}
	}
}
``````````
