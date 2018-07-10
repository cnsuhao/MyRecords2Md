---
title: Java读取PDF文字内容的方法
date: 2015-01-05 17:30:00
categories: "开发"
tags:
	- Java

---

**方法一：PDFBox**

（一个BSD许可下的源码开放项目）是一个为开发人员读取和创建PDF文档而准备的纯Java类库。它提供如下特性：

提取文本，包括Unicode字符。和Jakarta Lucene等文本搜索引擎的整合过程十分简单。加密/解密PDF文档。从PDF和XFDF格式中导入或导出表单数据。向已有PDF文档中追加内容。将一个PDF文档切分为多个文档，覆盖PDF文档。

官网：[http://pdfbox.apache.org/index.html ][http_pdfbox.apache.org_index.html] 截止当前最新版本1.8.8

```java

 package charlie.utils.pdf;

 

import java.io.BufferedWriter;

import java.io.File;

import java.io.FileInputStream;

import java.io.FileWriter;

import java.io.InputStream;

 

import org.apache.pdfbox.pdfparser.PDFParser;

import org.apache.pdfbox.pdmodel.PDDocument;

import org.apache.pdfbox.util.PDFTextStripper;

 

/**

 * @author CharlieChen

 * @DateTime 2015-1-5 上午9:55:38

 * @version 1.0

 */

public class PdfboxUtil {

 

   /**

    * @param args

    */

   public static void main(String[] args) {

      String pdfPath = "D:/temp/成交单-PDF格式.pdf";

      String txtfilePath = "D:/temp/成交单-PDF格式-pdfbox.txt";

      PdfboxUtil pdfutil = new PdfboxUtil();

      try {

         String content = pdfutil.getTextFromPdf(pdfPath);

         pdfutil.toTextFile(content, txtfilePath);

         System.out.println("Finished !");      

      } catch (Exception e) {

         e.printStackTrace();

      }

 

   }

  

   /**

    * 读取PDF文件的文字内容

    * @param pdfPath

    * @throws Exception

    */

   public String getTextFromPdf(String pdfPath) throws Exception {

      // 是否排序

      boolean sort = false;

      // 开始提取页数

      int startPage = 1;

      // 结束提取页数

      int endPage = Integer.MAX_VALUE;    

     

      String content = null;

      InputStream input = null;

      File pdfFile = new File(pdfPath);

      PDDocument document = null;

      try {

         input = new FileInputStream(pdfFile);

         // 加载 pdf 文档

         PDFParser parser = new PDFParser(input);

         parser.parse();

         document = parser.getPDDocument();

         // 获取内容信息

         PDFTextStripper pts = new PDFTextStripper();

         pts.setSortByPosition(sort);

         endPage = document.getNumberOfPages();

         System.out.println("Total Page: " + endPage);

         pts.setStartPage(startPage);

         pts.setEndPage(endPage);

         try {

            content = pts.getText(document);

         } catch (Exception e) {

            throw e;

         }

         System.out.println("Get PDF Content ...");

      } catch (Exception e) {

         throw e;

      } finally {

         if (null != input)

            input.close();

         if (null != document)

            document.close();

      }

     

      return content;

   }

 

   /**

    * 把PDF文件内容写入到txt文件中

    * @param pdfContent

    * @param filePath

    */

   public void toTextFile(String pdfContent,String filePath) {     

      try {

         File f = new File(filePath);

         if (!f.exists()) {

            f.createNewFile();

         }

         System.out.println("Write PDF Content to txt file ...");

         BufferedWriter output = new BufferedWriter(new FileWriter(f));

         output.write(pdfContent);

         output.close();

      } catch (Exception e) {

         e.printStackTrace();

      }

   }

}
```

另需要commons-logging和fontbox-1.8.8的jar包

pdfbox只能做到把文字分析出来，并无法很好的控制分析的顺序，格式，字体等信息。

排序sort为true后，PDF按行读取，保持了顺序，但是遇到分栏，分页就会需要额外处理

**方法二：iText**

用于能够快速产生PDF文档的一个java类库。通过iText不仅可以生成PDF或rtf的文档，而且可以将
XML、Html文件转化为PDF文件。支持文本，表格，图形的操作，可以方便的跟 Servlet 进行结合。

```java

package charlie.utils.pdf;

 

import java.io.FileOutputStream;

import java.io.IOException;

import java.io.PrintWriter;

import com.itextpdf.text.pdf.PdfReader;

import com.itextpdf.text.pdf.parser.PdfReaderContentParser;

import com.itextpdf.text.pdf.parser.PdfTextExtractor;

import com.itextpdf.text.pdf.parser.SimpleTextExtractionStrategy;

import com.itextpdf.text.pdf.parser.TextExtractionStrategy;

/**

 * @author CharlieChen

 * @DateTime 2015-1-5 上午11:43:26

 * @version 1.0

 */

public class ItextpdfUtil {

 

   /**

    * @param args

    */

   public static void main(String[] args) {

     

      String pdfPath = "D:/temp/成交单-PDF格式.pdf";

      String txtfilePath = "D:/temp/成交单-PDF格式-itext.txt";     

     

       //readPdfToTxt(pdfPath,txtfilePath); //调用读取方法

       System.out.println(readPdfToTxt(pdfPath));

       System.out.println("Finished !");

      

   }

 

   /**

    * 读取PDF文件内容到txt文件

    *

    * @param writer

    * @param pdfPath

    */

   private static void readPdfToTxt(String pdfPath,String txtfilePath) {

      // 读取pdf所使用的输出流

      PrintWriter writer = null;

      PdfReader reader = null;

      try {

         writer = new PrintWriter(new FileOutputStream(txtfilePath));

         reader = new PdfReader(pdfPath);

         int num = reader.getNumberOfPages();// 获得页数

         System.out.println("Total Page: " + num);

         String content = ""; // 存放读取出的文档内容

         for (int i = 1; i <= num; i++) {

            // 读取第i页的文档内容

            content += PdfTextExtractor.getTextFromPage(reader, i);

         }

         String[] arr = content.split("/n");

         for(int i=0;i<arr.length;i++){

            System.out.println(arr[i]);

            /*String[] childArr = arr[i].split(" ");

            for(int j=0;j<childArr.length;j++){

                System.out.println(childArr[j]);

            }*/

         }    

        

         //System.out.println(content);

        

         writer.write(content);// 写入文件内容

         writer.flush();

         writer.close();

      } catch (IOException e) {

         e.printStackTrace();

      }

   }

  

   /**

    * 读取pdf内容

    * @param pdfPath

    */

   private static String readPdfToTxt(String pdfPath) {

      PdfReader reader = null;

      StringBuffer buff = new StringBuffer();

      try {

         reader = new PdfReader(pdfPath);

         PdfReaderContentParser parser = new PdfReaderContentParser(reader);

         int num = reader.getNumberOfPages();// 获得页数

         TextExtractionStrategy strategy;

         for (int i = 1; i <= num; i++) {

            strategy = parser.processContent(i,

                   new SimpleTextExtractionStrategy());

            buff.append(strategy.getResultantText());

         }

      } catch (IOException e) {

         e.printStackTrace();

      } 

     

      return buff.toString();

   }

}

```

[http_pdfbox.apache.org_index.html]: http://pdfbox.apache.org/index.html%20%E6%88%AA%E6%AD%A2%E5%BD%93%E5%89%8D%E6%9C%80%E6%96%B0%E7%89%88%E6%9C%AC1.8.8
 *  **原文作者：** 三十的狼
 *  **原文链接：** http://www.360doc.com/content/18/0112/00/51091873_721208391.shtml
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。