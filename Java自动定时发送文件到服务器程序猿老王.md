---
title: Java自动定时发送文件到服务器
date: 2018-03-18 11:31:23
categories: "开发"
tags:
	- Java
	- 技术
	- 编程语言

---

//客户端代码

![Java自动定时发送文件到服务器][Java]

文件类名称: FileUploadController.java


``````````
/*** 把文件压缩成zip文件上传到服务器* @param oldFiles* @param zippath* @throws HttpException* @throws IOException*/public static void loadfile(String oldFiles, String zippath) throws IOException {HttpClient client = new HttpClient();File zipfile = new File(zippath);String URL = "http://127.0.0.1:8080/db/file";//上传地址PostMethod filePost = new PostMethod(URL);System.out.println("---准备压缩，文件名:" + zipfile.getName());client.getHttpConnectionManager().getParams().setConnectionTimeout(1000 * 30);// 设置超时30秒// 把文件夹压缩成压缩文件，如果压缩文件存在，会替换原文件FileUploadController.ZipMultiFile(oldFiles, zippath);FilePart filepart = new FilePart("upfile", new File(zippath));//下面是添加参数Part[] parts = {new StringPart("corp_code", "C11111"), new StringPart("fileName", "demo.zip"), filepart};MultipartRequestEntity mre = new MultipartRequestEntity(parts, filePost.getParams());filePost.setRequestEntity(mre);long s = System.currentTimeMillis();int status = client.executeMethod(filePost);long e = System.currentTimeMillis();System.out.println("----上传状态:" + status + ",耗时:" + (e - s) / 1000 + "秒");}
``````````

``````````
/*** 前面是文件夹，如:D:/JAVA/,后面是压缩后的文件路径和名称,如:D:11.zip 一次性压缩多个文件，文件存放至一个文件夹中** @param filepath* @param zippath*/public static void ZipMultiFile(String filepath, String zippath) {long s = System.currentTimeMillis();try {File file = new File(filepath);// 要被压缩的文件夹File zipFile = new File(zippath);InputStream input = null;ZipOutputStream zipOut = new ZipOutputStream(new FileOutputStream(zipFile));if (file.isDirectory()) {File[] files = file.listFiles();for (int i = 0; i < files.length; ++i) {input = new FileInputStream(files[i]);zipOut.putNextEntry(new ZipEntry(file.getName() + File.separator + files[i].getName()));int temp = 0;while ((temp = input.read()) != -1) {zipOut.write(temp);}input.close();}}zipOut.close();} catch (Exception e) {e.printStackTrace();}long e = System.currentTimeMillis();System.out.println("---压缩完成:" + zippath + ",耗时:" + (e - s) / 1000 + "秒");}
``````````

//下面���服务端接收文件的代码和定时任务的代码

``````````
/*** 服务器接收数据的方法* @param request* @return* @throws IOException*/@RequestMapping(value = "/db/file")@ResponseBodypublic String reciveFile(HttpServletRequest request) throws IOException, ServletException {String corp_code = request.getParameter("corp_code");//接收参数String fileName = request.getParameter("fileName");//获取文件名log.info("-----传递过来公司编号:" + corp_code + "---文件名:" + fileName);long s = System.currentTimeMillis();byte[] buffer = new byte[1024 * 1024];InputStream in = request.getPart("upfile").getInputStream();FileOutputStream out = new FileOutputStream("C:/" + fileName);int length = 0;while ((length = in.read(buffer)) > 0) {out.write(buffer, 0, length);}if (out != null) {out.close();}File file = new File("C:/" + fileName);long e = System.currentTimeMillis();log.info("-----写入完成:" + file.length() / 1024 / 1024 + "MB,耗时:" + (e - s) / 1000 + "秒");return "File";}
``````````

``````````
/** * 定时任务 实现方式1* @author server*/@Configuration@EnableScheduling // 标注启动定时任务 这里是：关闭状态，拿下注释就开启了public class SchudelTime {@Scheduled(fixedRate = 1000 * 60 * 3, initialDelay = 1000*5) // 每隔3分钟执行一次，延迟5秒执行public void updatePayRecords() {System.out.println("每隔1分钟执行一次，延迟10秒执行: The time is now " + new Date().toLocaleString());try { //参数：������压缩上传的文件夹 压缩后的压缩包FileUploadController.loadfile("E:/JAVA/","E:vsalw.zip");} catch (HttpException e) {// TODO Auto-generated catch blocke.printStackTrace();} catch (IOException e) {// TODO Auto-generated catch blocke.printStackTrace();}}定时任务实现方法有很多种。这里仅仅是一种。【喜欢这类文章可以关注我，我会继续发类似技术文章，谢谢】
``````````


[Java]: /pro/os/crawler/AEUM-VAMI-AQIN.jpg
 *  **原文作者：** 程序猿老王
 *  **原文链接：** https://www.toutiao.com/item/6533043600572285443/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。