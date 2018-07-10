---
title: Java使用google的thumbnailator工具对图片压缩水印等做处理
date: 2017-11-07 10:08:56
categories: "开发"
tags:
	- Java
	- Google
	- 技术
	- 编程语言

---

# 今天给大家分享一个非常好用的工具thumbnailator。Thumbnailator是一个非常好的图片开源工具 #

# 使用方法：    #

# 在pom中加入以下jar包 #

    <!-- 图片缩略图 图片压缩 水印 start--><dependency><groupId>net.coobird</groupId><artifactId>thumbnailator</artifactId><version>0.4.8</version></dependency><!-- 图片缩略图 图片压缩 水印 end-->

# 然后压缩和水印 只需要一行代码搞定 #

    package com.shallowmemory.test;import net.coobird.thumbnailator.Thumbnails;import net.coobird.thumbnailator.geometry.Positions;import javax.imageio.ImageIO;import java.awt.image.BufferedImage;import java.io.File;import java.io.IOException;/*** Created by HONGLINCHEN on 2017/10/31 11:00* 图片压缩* @author HONGLINCHEN* @since JDK 1.8*/public class ImgCompress {public static void main(String[] args) throws IOException {//压缩图片 第一个参数是原图路径 后面那个路径是压缩以后的输出路径Thumbnails.of("C:\Users\HONGLINCHEN\Desktop\23.jpg").size(600,600).outputQuality(0.8f).toFile("C:\Users\HONGLINCHEN\Desktop\2.jpg");//给图片加水印BufferedImage watermarkImage = ImageIO.read(new File("C:\Users\HONGLINCHEN\Desktop\1.jpg"));//第一个参数是水印的位置；第二个参数是水印图片的缓存数据；第三个参数是透明度。Thumbnails.of("C:\Users\HONGLINCHEN\Desktop\23.jpg").scale(0.8).watermark(Positions.BOTTOM_RIGHT, watermarkImage, 0.5f).toFile("C:\Users\HONGLINCHEN\Desktop\3.jpg");}}
 *  **原文作者：** 专注JavaWeb开发
 *  **原文链接：** https://www.toutiao.com/item/6485488816491610637/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。