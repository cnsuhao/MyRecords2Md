---
title: Java实现根据指定的参数值、模板，生成 word 文档
date: 2017-11-28 07:00:52
categories: "开发"
tags:
	- Java
	- Word
	- 技术
	- 编程语言

---

直接上代码：  


import java.io.ByteArrayInputStream;

import java.io.FileInputStream;

import java.io.IOException;

import java.io.InputStream;

import java.util.Iterator;

import java.util.List;

import java.util.Map;

import java.util.Map.Entry;

import org.apache.poi.POIXMLDocument;

import org.apache.poi.openxml4j.opc.OPCPackage;

import org.apache.poi.xwpf.usermodel.XWPFParagraph;

import org.apache.poi.xwpf.usermodel.XWPFRun;

import org.apache.poi.xwpf.usermodel.XWPFTable;

import org.apache.poi.xwpf.usermodel.XWPFTableCell;

import org.apache.poi.xwpf.usermodel.XWPFTableRow;

public class WordUtil \{

/\*\*

\* 根据指定的参数值、模板，生成 word 文档

\* @param param 需要替换的变量

\* @param template 模板

\*/

public static CustomXWPFDocument generateWord(Map<String, Object> param, String template) \{

CustomXWPFDocument doc = null;

try \{

OPCPackage pack = POIXMLDocument.openPackage(template);

doc = new CustomXWPFDocument(pack);

if (param != null && param.size() > 0) \{

//处理段落

List<XWPFParagraph> paragraphList = doc.getParagraphs();

processParagraphs(paragraphList, param, doc);

//处理表格

Iterator<XWPFTable> it = doc.getTablesIterator();

while (it.hasNext()) \{

XWPFTable table = it.next();

List<XWPFTableRow> rows = table.getRows();

for (XWPFTableRow row : rows) \{

List<XWPFTableCell> cells = row.getTableCells();

for (XWPFTableCell cell : cells) \{

List<XWPFParagraph> paragraphListTable = cell.getParagraphs();

processParagraphs(paragraphListTable, param, doc);

\}

\}

\}

\}

\} catch (Exception e) \{

e.printStackTrace();

\}

return doc;

\}

/\*\*

\* 处理段落

\* @param paragraphList

\*/

public static void processParagraphs(List<XWPFParagraph> paragraphList,Map<String, Object> param,CustomXWPFDocument doc)\{

if(paragraphList != null && paragraphList.size() > 0)\{

for(XWPFParagraph paragraph:paragraphList)\{

List<XWPFRun> runs = paragraph.getRuns();

for (XWPFRun run : runs) \{

String text = run.getText(0);

if(text != null)\{

boolean isSetText = false;

for (Entry<String, Object> entry : param.entrySet()) \{

String key = entry.getKey();

if(text.indexOf(key) != -1)\{

isSetText = true;

Object value = entry.getValue();

if (value instanceof String) \{//文本替换

text = text.replace(key, value.toString());

\} else if (value instanceof Map) \{//图片替换

text = text.replace(key, "");

Map pic = (Map)value;

int width = Integer.parseInt(pic.get("width").toString());

int height = Integer.parseInt(pic.get("height").toString());

int picType = getPictureType(pic.get("type").toString());

byte\[\] byteArray = (byte\[\]) pic.get("content");

ByteArrayInputStream byteInputStream = new ByteArrayInputStream(byteArray);

try \{

int ind = doc.addPicture(byteInputStream,picType);

doc.createPicture(ind, width , height,paragraph);

\} catch (Exception e) \{

e.printStackTrace();

\}

\}

\}

\}

if(isSetText)\{

run.setText(text,0);

\}

\}

\}

\}

\}

\}

/\*\*

\* 根据图片类型，取得对应的图片类型代码

\* @param picType

\* @return int

\*/

private static int getPictureType(String picType)\{

int res = CustomXWPFDocument.PICTURE\_TYPE\_PICT;

if(picType != null)\{

if(picType.equalsIgnoreCase("png"))\{

res = CustomXWPFDocument.PICTURE\_TYPE\_PNG;

\}else if(picType.equalsIgnoreCase("dib"))\{

res = CustomXWPFDocument.PICTURE\_TYPE\_DIB;

\}else if(picType.equalsIgnoreCase("emf"))\{

res = CustomXWPFDocument.PICTURE\_TYPE\_EMF;

\}else if(picType.equalsIgnoreCase("jpg") || picType.equalsIgnoreCase("jpeg"))\{

res = CustomXWPFDocument.PICTURE\_TYPE\_JPEG;

\}else if(picType.equalsIgnoreCase("wmf"))\{

res = CustomXWPFDocument.PICTURE\_TYPE\_WMF;

\}

\}

return res;

\}

/\*\*

\* 将输入流中的数据写入字节数组

\* @param in

\* @return

\*/

public static byte\[\] inputStream2ByteArray(InputStream in,boolean isClose)\{

byte\[\] byteArray = null;

try \{

int total = in.available();

byteArray = new byte\[total\];

in.read(byteArray);

\} catch (IOException e) \{

e.printStackTrace();

\}finally\{

if(isClose)\{

try \{

in.close();

\} catch (Exception e2) \{

System.out.println("关闭流失败");

\}

\}

\}

return byteArray;

\}

\}

![Java实现根据指定的参数值、模板，生成 word 文档][Java_ word]

网络配图


[Java_ word]: /pro/os/crawler/VNMA-NBMZ-RBYY.jpg
 *  **原文作者：** 历史使人明智
 *  **原文链接：** https://www.toutiao.com/item/6492919832386732557/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。