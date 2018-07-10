---
title: 在1分钟内创建人脸检测REST API
date: 2018-05-21 16:01:01
categories: "开发"
tags:
	- Git
	- 软件
	- 编程语言
	- JSON
	- Python

---

![在1分钟内创建人脸检测REST API][1_REST API]

# 安装 #

使用python 3.5在Ubuntu 16.04中测试了人脸检测应用程序。点击终端下面的命令安装应用程序。

> git clone https://github.com/neerajshukla1911/face-detection-app.git
> 
> cd face-detection-app
> 
> pip install -r requirements.tx
> 
> git clone https://github.com/davisking/dlib.git
> 
> python setup.py install --yes USE\_AVX\_INSTRUCTIONS

# 运行服务器 #

以下命令将在端口8882上启动人脸检测服务器。可以在app.py文件中修改端口。

> python app.py

# REST API详细信息 #

以下是人脸检测REST API的详细信息。

> \- Request Type: form-data
> 
> \- Request method: post
> 
> \- API: http://localhost:8882/face-detection
> 
> \- Request Parameter:
> 
> \- file: file object (Required)
> 
> \- return\_predicted\_image: Return base64 string of image with detected faces bounding boxes. Value of parameter can true/false (optional)
> 
> \- Response: Returns list of coordinates of detected faces. Below is sample response.
> 
> \{
> 
> "predictions": \[
> 
> \{
> 
> "box": \{
> 
> "xmax": 394,
> 
> "xmin": 305,
> 
> "ymax": 166,
> 
> "ymin": 76
> 
> \}
> 
> \}
> 
> \]
> 
> \}
> 
> \- Request Type: application/json
> 
> \- Request method: post
> 
> \- API: http://localhost:8882/face-detection
> 
> \- Request Parameter:
> 
> \- image\_url: "image url of image" (Required)
> 
> \- return\_predicted\_image: Return base64 string of image with detected faces bounding boxes. Value of parameter can true/false (optional)
> 
> \- Response: Returns list of coordinates of detected faces. Below is sample response.
> 
> \{
> 
> "predictions": \[
> 
> \{
> 
> "box": \{
> 
> "xmax": 394,
> 
> "xmin": 305,
> 
> "ymax": 166,
> 
> "ymin": 76
> 
> \}
> 
> \}
> 
> \]
> 
> \}

# 用法 #

您可以使用任何http客户端在http://localhost:8882/face-detection 发送post请求。更改文件路径到您的图像文件路径。

> curl -F "file=@/home/neeraj/2.jpg" localhost:8882/face-detection
> 
> curl -F "file=@/home/neeraj/2.jpg" -F "return\_predicted\_image=true" localhost:8882/face-detection
> 
> curl --header "Content-Type: application/json" --request POST --data '\{"image\_url":"https://d2zv4gzhlr4ud6.cloudfront.net/media/pictures/tagged\_items/540x0/119\_CFM04BL976/1.jpg"\}' localhost:8882/face-detection
> 
> curl --header "Content-Type: application/json" --request POST --data '\{"image\_url":"https://d2zv4gzhlr4ud6.cloudfront.net/media/pictures/tagged\_items/540x0/119\_CFM04BL976/1.jpg", "return\_predicted\_image": true\}' localhost:8882/face-detection


[1_REST API]: /pro/os/crawler/2AVV-IERB-6NU2.jpg
 *  **原文作者：** 不靠谱的猫
 *  **原文链接：** https://www.toutiao.com/item/6557941161158246916/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。