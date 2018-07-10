---
title: Java做验证码识别 识别率甚高 码农必学
date: 2017-08-15 09:57:47
categories: "开发"
tags:
	- 网络爬虫
	- 程序员
	- Java
	- 技术
	- 编程语言

---

小编是一名Java程序员,然后工作中需要使用爬虫技术,去做数据抓取,但是抓取的话需要登陆,并含有验证码，所以小编也是在帖子关注了下相关技术，然后发现这个解决方案非常棒。

# 代码 #

package com.demo.bobo;

import java.awt.image.BufferedImage;

import java.io.File;

import java.io.FileOutputStream;

import java.io.IOException;

import java.io.InputStream;

import java.io.OutputStream;

import java.util.Date;

import javax.imageio.ImageIO;

import org.apache.commons.httpclient.HttpClient;

import org.apache.commons.httpclient.HttpStatus;

import org.apache.commons.httpclient.methods.GetMethod;

import org.apache.commons.io.IOUtils;

import com.asprise.ocr.Ocr;

import com.asprise.util.ocr.OCR;

public class ReadImg \{

public static void main(String\[\] args) throws IOException \{

HttpClient httpClient = new HttpClient();

GetMethod getMethod = new GetMethod("http://dz.bjjtgl.gov.cn/service/checkCode.do");

//GetMethod getMethod = new GetMethod("https://dynamic.12306.cn/otsweb/passCodeAction.do?rand=sjrand");

int statusCode = httpClient.executeMethod(getMethod);

if (statusCode != HttpStatus.SC\_OK) \{

System.err.println("Method failed: " + getMethod.getStatusLine());

return ;

\}

String picName = "F:\\img\\";

File filepic=new File(picName);

if(!filepic.exists())

filepic.mkdir();

File filepicF=new File(picName+new Date().getTime() + ".jpg");

InputStream inputStream = getMethod.getResponseBodyAsStream();

OutputStream outStream = new FileOutputStream(filepicF);

IOUtils.copy(inputStream, outStream);

outStream.close();

Ocr.setUp(); // one time setup

Ocr ocr = new Ocr(); // create a new OCR engine

ocr.startEngine("eng", Ocr.SPEED\_FASTEST); // English

String s = ocr.recognize(new File\[\] \{filepicF\},Ocr.RECOGNIZE\_TYPE\_TEXT, Ocr.OUTPUT\_FORMAT\_PLAINTEXT);

System.out.println("Result: " + s);

System.out.println("图片文字为:" + s.replace(",", "").replace("i", "1").replace(" ", "").replace("'", "").replace("o", "0").replace("O", "0").replace("g", "6").replace("B", "8").replace("s", "5").replace("z", "2"));

// ocr more images here ...

ocr.stopEngine();

\}

\}

![Java做验证码识别 识别率甚高 码农必学][Java_ _]

![Java做验证码识别 识别率甚高 码农必学][Java_ _ 1]

# 架包引用Maven    #

    <dependency>

# JAR架包官网地址 #

http://asprise.com/royalty-free-library/java-ocr-api-overview.html


[Java_ _]: /pro/os/crawler/MRJB-YR7J-FMIN.jpg
[Java_ _ 1]: /pro/os/crawler/AAEI-BVMM-3YNN.jpg
 *  **原文作者：** David
 *  **原文链接：** https://www.toutiao.com/item/6454314788456497678/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。