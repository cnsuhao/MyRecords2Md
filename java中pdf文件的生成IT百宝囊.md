---
title: java中pdf文件的生成
date: 2018-06-05 23:00:21
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言
	- HTML

---

开发项目中需要使用java生成pdf，简单写了个代码，记录一下，有需要的朋友可以拿去。

# 基本思路 #

利用freemark模板引擎来生成html，然后通过java中的itext包来完成pdf的生成。

# maven包依赖 #

依然通过mavean来管理jar的依赖，下面直接上代码:

``````````
<dependency> <groupId>com.lowagie</groupId> <artifactId>itext</artifactId> <version>4.2.2</version> <type>pom</type></dependency>
``````````

# freemark生成html的配置 #

``````````
public class FreemarkerConfiguration {private static Configuration config = null; static{  config = new Configuration();  config.setClassForTemplateLoading(FreemarkerConfiguration.class, "template");  config.setTemplateUpdateDelay(0);  }public static Configuration getConfiguation(){  return config;  } }
``````````

# 利用freemarker生成静���html #

``````````
public class HtmlHelper { public static String generate(String template, Map params) throws Exception{  Configuration config = FreemarkerConfiguration.getConfiguation(); config.setDefaultEncoding("UTF-8");  Template tp = config.getTemplate(template);  StringWriter stringWriter = new StringWriter();  BufferedWriter writer = new BufferedWriter(stringWriter);  tp.setEncoding("UTF-8");  tp.process(params, writer);  String htmlStr = stringWriter.toString();  writer.flush();  writer.close();  return htmlStr; }}
``````````

# 将html转换为pdf #

``````````
public class PdfHelper { public static void generate() throws Exception  {  OutputStream os = null;  String htmlStr;  Map<String, Object> params = new HashMap<String, Object>();  Map data = new HashMap();  try {  data.put("projects","生成PDF");   //通过freemaker模板生成html  htmlStr = HtmlHelper.generate("test.ftl", data);  String appPath = PdfHelper.class.getResource("").getPath()+"dir"; System.out.println(appPath); ITextRenderer renderer = new ITextRenderer();  renderer.setDocumentFromString(htmlStr);   // 解决中文支持问题  ITextFontResolver fontResolver = renderer.getFontResolver();  fontResolver.addFont(FreemarkerConfiguration.class.getResource("template")+File.separator+"SIMSUN.TTC", BaseFont.IDENTITY_H, BaseFont.NOT_EMBEDDED);   //生成pdf文件  os = new FileOutputStream(PdfHelper.class.getResource("dir").toURI().getPath()+File.separator+"test.pdf"); renderer.layout();  renderer.createPDF(os, true);   os.flush();  } catch (Exception e) {  e.printStackTrace();  }finally {  if (null != os) {  try {  os.close();  } catch (IOException e) {  e.printStackTrace();  }  }  }   } }
``````````

# 关于汉字问题的补充 #

按照上面的方法，生成的pdf文件中如果含有汉字，汉字将依然看不到，如何解决呢，关键是在freemark的模板文件中设置如下代码：

``````````
<body style = "font-family: SimSun;" >
``````````
 *  **原文作者：** IT������囊
 *  **原文链接：** https://www.toutiao.com/item/6563615497613476360/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。