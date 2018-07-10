---
title: 使用Java获取文件类型
date: 2016-09-17 22:40:51
categories: "开发"
tags:
	- Java

---

##### Using Java 7 #####

[Files.html\#probeContentType][Files.html_probeContentType] 

``````````
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class Test {
  public static void main(String[] args) throws IOException {
    Path source = Paths.get("c:/temp/0multipage.tif");
    System.out.println(Files.probeContentType(source));
    // output : image/tiff
  }
}
``````````

The default implementation is OS-specific and not very complete. It's possible to register a better detector, like for example Apache Tika, see  [Transparently improve Java 7 mime-type recognition with Apache Tika][] .

##### Using javax.activation.MimetypesFileTypeMap #####

activation.jar  is required, it can be downloaded from  http://java.sun.com/products/javabeans/glasgow/jaf.html .

The MimetypesFileMap class is used to map a File to a Mime Type. Mime types supported are defined in a ressource file inside the activation.jar.

``````````
import javax.activation.MimetypesFileTypeMap;
import java.io.File;

class GetMimeType {
  public static void main(String args[]) {
    File f = new File("gumby.gif");
    System.out.println("Mime Type of " + f.getName() + " is " +
                         new MimetypesFileTypeMap().getContentType(f));
    // expected output :
    // "Mime Type of gumby.gif is image/gif"
  }
}
``````````

The built-in mime-type list is very limited but a mechanism is available to add very easily more Mime Types/extensions.

The MimetypesFileTypeMap looks in various places in the user's system for MIME types file entries. When requests are made to search for MIME types in the MimetypesFileTypeMap, it searches MIME types files in the following order:

1.  Programmatically added entries to the MimetypesFileTypeMap instance.
2.  The file .mime.types in the user's home directory.
3.  The file <java.home>/lib/mime.types.
4.  The file or resources named META-INF/mime.types.
5.  The file or resource named META-INF/mimetypes.default (usually found only in the activation.jar file).

This method is interesting when you need to deal with incoming files with the filenames normalized. The result is very fast because only the extension is used to guess the nature of a given file.

##### Using java.net.URL #####

Warning : this method is  very slow! .

Like the above method a match is done with the extension. The mapping between the extension and the mime-type is defined in the file \[jre\_home\]\\lib\\content-types.properties

``````````
import java.net.*;

public class FileUtils{
  public static String getMimeType(String fileUrl)
    throws java.io.IOException, MalformedURLException
  {
    String type = null;
    URL u = new URL(fileUrl);
    URLConnection uc = null;
    uc = u.openConnection();
    type = uc.getContentType();
    return type;
  }

  public static void main(String args[]) throws Exception {
    System.out.println(FileUtils.getMimeType("file://c:/temp/test.TXT"));
    // output :  text/plain
  }
}
``````````

A note from R. Lovelock :

``````````
I was trying to find the best way of getting the mime type of a file
and found your sight very useful. However I have now found a way of
getting the mime type using URLConnection that isn't as slow as the
way you describe.
``````````

``````````
import java.net.FileNameMap;
import java.net.URLConnection;

public class FileUtils {

  public static String getMimeType(String fileUrl)
      throws java.io.IOException
    {
      FileNameMap fileNameMap = URLConnection.getFileNameMap();
      String type = fileNameMap.getContentTypeFor(fileUrl);

      return type;
    }

    public static void main(String args[]) throws Exception {
      System.out.println(FileUtils.getMimeType("file://c:/temp/test.TXT"));
      // output :  text/plain
    }
  }
``````````

##### Using Apache Tika #####

Tika is subproject of Lucene, a search engine. It is a toolkit for  detecting  and  extracting metadata and structured text content  from various documents using existing parser libraries.

This package is very up-to-date regarding the filetypes supported, Office 2007 formats are supported (docs/pptx/xlsx/etc...).

[Apache Tika][]

Tika has a lot of dependencies ... almost 20 jars ! But it can do a lot more than detecting filetype. For example, you can parse a PDF or DOC to extract the text and the metadata very easily.

``````````
import java.io.File;
import java.io.FileInputStream;

import org.apache.tika.metadata.Metadata;
import org.apache.tika.parser.AutoDetectParser;
import org.apache.tika.parser.Parser;
import org.apache.tika.sax.BodyContentHandler;
import org.xml.sax.ContentHandler;

public class Main {

    public static void main(String args[]) throws Exception {

    FileInputStream is = null;
    try {
      File f = new File("C:/Temp/mime/test.docx");
      is = new FileInputStream(f);

      ContentHandler contenthandler = new BodyContentHandler();
      Metadata metadata = new Metadata();
      metadata.set(Metadata.RESOURCE_NAME_KEY, f.getName());
      Parser parser = new AutoDetectParser();
      // OOXMLParser parser = new OOXMLParser();
      parser.parse(is, contenthandler, metadata);
      System.out.println("Mime: " + metadata.get(Metadata.CONTENT_TYPE));
      System.out.println("Title: " + metadata.get(Metadata.TITLE));
      System.out.println("Author: " + metadata.get(Metadata.AUTHOR));
      System.out.println("content: " + contenthandler.toString());
    }
    catch (Exception e) {
      e.printStackTrace();
    }
    finally {
        if (is != null) is.close();
    }
  }
}
``````````

You can download  [here][]  a ZIP containing the required jars if you want to check it out.

##### Using JMimeMagic #####

Checking the file extension is not a very strong way to determine the file type. A more robust solution is possible with the  [JMimeMagic library][] . JMimeMagic is a Java library (LGLP licence) that retrieves file and stream mime types by checking magic headers.

``````````
// snippet for JMimeMagic lib
//     http://sourceforge.net/projects/jmimemagic/

Magic parser = new Magic() ;
// getMagicMatch accepts Files or byte[],
// which is nice if you want to test streams
MagicMatch match = parser.getMagicMatch(new File("gumby.gif"));
System.out.println(match.getMimeType()) ;
``````````

Thanks to Jean-Marc Autexier and sygsix for the tip! 

##### Using mime-util #####

Another tool is  [mime-util][] . This tool can detect using the file extension or the magic header technique.

``````````
import eu.medsea.mimeutil.MimeUtil;

public class Main {
    public static void main(String[] args) {
        MimeUtil.registerMimeDetector("eu.medsea.mimeutil.detector.MagicMimeMimeDetector");
        File f = new File ("c:/temp/mime/test.doc");
        Collection<?> mimeTypes = MimeUtil.getMimeTypes(f);
        System.out.println(mimeTypes);
        //  output : application/msword
    }
}
``````````

The nice thing about mime-util is that it is very lightweight. Only 1 dependency with  [slf4j][] 

##### Using Droid #####

DROID (Digital Record Object Identification) is a software tool to perform automated batch identification of file formats.

DROID uses internal and external signatures to identify and report the specific file format versions of digital files. These signatures are stored in an XML signature file, generated from information recorded in the PRONOM technical registry. New and updated signatures are regularly added to PRONOM, and DROID can be configured to automatically download updated signature files from the PRONOM website via web services.

It can be invoked from two interfaces, a Java Swing GUI or a command line interface.

http://droid.sourceforge.net/wiki/index.php/Introduction

##### Aperture framework #####

Aperture is an open source library and framework for crawling and indexing information sources such as file systems, websites and mail boxes.

The Aperture code consists of a number of related but independently usable parts:

 *  Crawling of information sources: file systems, websites, mail boxes
 *  MIME type identification
 *  Full-text and metadata extraction of various file formats
 *  Opening of crawled resources

For each of these parts, a set of APIs has been developed and a number of implementations is provided.

http://aperture.wiki.sourceforge.net/Overview


[Files.html_probeContentType]: http://docs.oracle.com/javase/7/docs/api/java/nio/file/Files.html#probeContentType%28java.nio.file.Path%29
[Transparently improve Java 7 mime-type recognition with Apache Tika]: http://odoepner.wordpress.com/2013/07/29/transparently-improve-java-7-mime-type-recognition-with-apache-tika/
[Apache Tika]: http://lucene.apache.org/tika/
[here]: http://www.rgagnon.com/examples/demo-tika-libs.zip
[JMimeMagic library]: http://sourceforge.net/projects/jmimemagic/
[mime-util]: http://sourceforge.net/projects/mime-util
[slf4j]: http://www.slf4j.org/
 *  **原文作者：** kimsoft
 *  **原文链接：** https://blog.csdn.net/kimsoft/article/details/52566412
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。