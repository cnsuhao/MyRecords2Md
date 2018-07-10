---
title: Java实现读取pdf文件内容(how to read pdf in java)
date: 2017-10-07 12:27:38
categories: "开发"
tags:
	- Java
	- java

---

本文将利用pdfbox实现pdf文件内容的读取。

环境：

1. eclipse oxygen

2. maven 3.3

3. jdk 1.8

1.通过eclipse创建maven项目，最终项目目录如下：

![YIE2-UR3M-ZJIE.jpg][]


2.pom.xml

``````````
<dependencies>
	<dependency>
	        <groupId>org.apache.pdfbox</groupId>
	        <artifactId>pdfbox</artifactId>
	        <version>2.0.6</version>
	</dependency>
  </dependencies>
``````````

3. PDF 打印

从pdf文件中提取文本进行打印输出

ReadPdf.java

``````````
package org.thinkingingis;

import java.io.File;
import java.io.IOException;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.encryption.InvalidPasswordException;
import org.apache.pdfbox.text.PDFTextStripper;
import org.apache.pdfbox.text.PDFTextStripperByArea;

public class ReadPdf {
	public static void main(String[] args) {
		
		try (PDDocument document = PDDocument.load(new File("/Users/gisboy/Desktop/daodejing.pdf"))) {
			
			document.getClass();
			
			if(!document.isEncrypted()) {
				PDFTextStripperByArea stripper = new PDFTextStripperByArea();
				stripper.setSortByPosition(true);
				PDFTextStripper tStripper = new PDFTextStripper();
				
				String pdfFileInText = tStripper.getText(document);
				
				String[] lines = pdfFileInText.split("\\r?\\n");
				for(String line : lines) {
					System.out.println(line);
				}
				
			}
			
		} catch (InvalidPasswordException e) {
			
			e.printStackTrace();
		} catch (IOException e) {
			
			e.printStackTrace();
		} 
		
	}

}
``````````

4.结果如下

![3IVR-N2EM-Q7NR.jpg][]







[YIE2-UR3M-ZJIE.jpg]: /pro/os/crawler/YIE2-UR3M-ZJIE.jpg
[3IVR-N2EM-Q7NR.jpg]: /pro/os/crawler/3IVR-N2EM-Q7NR.jpg
 *  **原文作者：** gisboygogogo
 *  **原文链接：** https://blog.csdn.net/gisboygogogo/article/details/78168976?locationNum=6&fps=1
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。