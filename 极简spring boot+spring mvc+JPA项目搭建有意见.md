---
title: 极简spring boot+spring mvc+JPA项目搭建
date: 2017-11-14 15:04:50
categories: "开发"
tags:
	- Tomcat
	- 技术
	- IntelliJ IDEA
	- MySQL

---

1、打开网址http://start.spring.io，

点击链接“Switch to the full version.”来到页面如下，填入相应的项目信息，并在页面下方选择Web、JPA、MySQL三个依赖，如下图

![极简spring boot+spring mvc+JPA项目搭建][spring boot_spring mvc_JPA]

2、点击Generate Project按钮，下载空项目文件。空项目文件是一个zip压缩包，解压到任意位置，用IntelliJ IDEA打开，

![极简spring boot+spring mvc+JPA项目搭建][spring boot_spring mvc_JPA 1]

3、从项目面板中打开pom.xml，找到

<dependency>

<groupId>org.springframework.boot</groupId>

<artifactId>spring-boot-starter-tomcat</artifactId>

<scope>provided</scope>

</dependency>

部分，将<scope>provided</scope>删除掉

4、创建一个数据库，如MySQL

5、从项目面板中打开application.properties，写入数据库连接信息

\#\# spring.jpa.hibernate.ddl-auto=update

spring.jpa.hibernate.ddl-auto=create

spring.datasource.url=jdbc:mysql://localhost:3306/demo

spring.datasource.username=root

spring.datasource.password=1111

6、创建一个实体类

package com.example.demo.entity;

import javax.persistence.Entity;

import javax.persistence.GeneratedValue;

import javax.persistence.GenerationType;

import javax.persistence.Id;

@Entity

public class User \{

@Id

@GeneratedValue(strategy = GenerationType.AUTO)

private Integer id;

private String name;

private String email;

public Integer getId() \{

return id;

\}

public void setId(Integer id) \{

this.id = id;

\}

public String getName() \{

return name;

\}

public void setName(String name) \{

this.name = name;

\}

public String getEmail() \{

return email;

\}

public void setEmail(String email) \{

this.email = email;

\}

\}

7、创建一个repository，

package com.example.demo.repository;

import com.example.demo.entity.User;

import org.springframework.data.repository.CrudRepository;

public interface UserRepository extends CrudRepository<User, Integer> \{

\}

8、创建一个controller，

package com.example.demo.controller;

import com.example.demo.entity.User;

import com.example.demo.repository.UserRepository;

import org.springframework.beans.factory.annotation.Autowired;

import org.springframework.stereotype.Controller;

import org.springframework.web.bind.annotation.GetMapping;

import org.springframework.web.bind.annotation.RequestMapping;

import org.springframework.web.bind.annotation.RequestParam;

import org.springframework.web.bind.annotation.ResponseBody;

@Controller

@RequestMapping(path = "/user")

public class UserController \{

@Autowired

private UserRepository userRepository;

@GetMapping(path = "/list")

public @ResponseBody Iterable<User> list() \{

return userRepository.findAll();

\}

@GetMapping(path = "add")

public @ResponseBody String add(@RequestParam String name,

@RequestParam String email) \{

User user = new User();

user.setName(name);

user.setEmail(email);

userRepository.save(user);

return "done";

\}

\}

9、启动项目，浏览器打开http://localhost:8080/user/add?name=jack&email=123atabc\_com ，即可在数据库中插入一条记录，浏览器打开http://localhost:8080/user/list即可查看所有的结果


[spring boot_spring mvc_JPA]: /pro/os/crawler/VBQY-VRF7-RVB2.jpg
[spring boot_spring mvc_JPA 1]: /pro/os/crawler/MRIZ-IMJA-FQFA.jpg
 *  **原文作者：** 有意见
 *  **原文链接：** https://www.toutiao.com/item/6488161791690932749/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。