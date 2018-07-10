---
title: Google发布新Tensorflow物体识别API
date: 2017-08-31 14:34:48
categories: "开发"
tags:
	- Google
	- 可视化
	- GitHub
	- GPU
	- 大数据

---

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API]

大数据文摘作品，转载具体要求见文末

作者 | Priya Dwivedi

编译 | Lisa，Saint，Aileen

做图像识别有很多不同的途径。谷歌最近发布了一个使用Tensorflow的物体识别API，让计算机视觉在各方面都更进了一步。

这篇文章将带你测试这个新的API，并且把它应用在youtube上（可以在GitHub上获取用到的全部代码

https://github.com/priya-dwivedi/Deep-Learning/blob/master/Object\_Detection\_Tensorflow\_API.ipynb），结果如下：

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API 1]

API概述

这个API是用COCO（文本中的常见物体）数据集（http://mscoco.org/）训练出来的。这是一个大约有30万张图像、90种最常见物体的数据集。物体的样本包括：

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API 2]

COCO数据集的一些物体种类

这个API提供了5种不同的模型，使用者可以通过设置不同检测边界范围来平衡运行速度和准确率。

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API 3]

上图中的mAP（平均精度）是检测边界框的准确率和回召率的乘积。这是一个很好的混合测度，在评价模型对目标物体的敏锐度和它是否能很好的避免虚假目标中非常好用。mAP值越高，模型的准确度越高，但运行速度会相应下降。

（想要了解更多跟模型有关的知识https://github.com/tensorflow/models/blob/477ed41e7e4e8a8443bc633846eb01e2182dc68a/object\_detection/g3doc/detection\_model\_zoo.md）

实测时间

我决定使用最轻量级的模型（ssd\_mobilenet）。主要步骤如下：

1. 下载一个打包模型(.pb-protobuf)并把它载入缓存

2. 使用内置的辅助代码来载入标签，类别，可视化工具等等。

3. 建立一个新的会话，在图片上运行模型。

总体来说步骤非常简单。而且这个API文档还提供了一些能运行这些主要步骤的Jupyter文档——

https://github.com/tensorflow/models/blob/master/object\_detection/object\_detection\_tutorial.ipynb

这个模型在实例图像上表现得相当出色（如下图）：

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API 4]

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API 5]

更进一步——在视频上运行上

接下来我打算在视频上尝试这个API。我使用了Python moviepy库，主要步骤如下：

 *  首先，使用VideoFileClip函数从视频中提取图像；
 *  然后使用fl\_image函数在视频中提取图像，并在上面应用物体识别API。fl\_image是一个很有用的函数，可以提取图像并把它替换为修改后的图像。通过这个函数就可以实现在每个视频上提取图像并应用物体识别；
 *  最后，把所有处理过的图像片段合并成一个新视频。

对于3-4秒的片段，这个程序需要花费大概1分钟的时间来运行。但鉴于我们使用的是一个载入缓存的模型，而且没有使用GPU，我们实现的效果还是很惊艳的！很难相信只用这么一点代码，就可以以很高的准确率检测并且在很多常见物体上画出边界框。

当然，我们还是能看到有一些表现有待提升。比如下面的例子。这个视频里的鸟完全没有被检测出来。

![Google发布新Tensorflow物体识别API][Google_Tensorflow_API 6]

再进一步，继续探索

几个进一步探索这个API的想法：

 *  尝试一些准确率更高但成本也更高的模型，看看他们有什么不同；
 *  寻找加速这个API的方法，这样它就可以被用于车载装置上进行实时物体检测；
 *  谷歌也提供了一些技能来应用这些模型进行传递学习。例如，载入打包模型后添加一个带有不同图像类别的输出层。

参考文献：

 *  Google Tensorflow Object Detection Github

 *  COCO dataset

原文链接：https://medium.com/towards-data-science/is-google-tensorflow-object-detection-api-the-easiest-way-to-implement-image-recognition-a8bd1f500ea0


[Google_Tensorflow_API]: /pro/os/crawler/FEBQ-QF6V-7NAR.jpg
[Google_Tensorflow_API 1]: /pro/os/crawler/ERJ2-QAV3-AABE.gif
[Google_Tensorflow_API 2]: /pro/os/crawler/VRN6-JJMZ-FI3Y.jpg
[Google_Tensorflow_API 3]: /pro/os/crawler/AUF2-YYNU-UBYU.jpg
[Google_Tensorflow_API 4]: /pro/os/crawler/R3YB-EJUY-FRBY.jpg
[Google_Tensorflow_API 5]: /pro/os/crawler/QFFQ-VEQY-Q2AB.jpg
[Google_Tensorflow_API 6]: /pro/os/crawler/ZYQR-QYZI-YA6R.gif
 *  **原文作者：** 大数据文摘
 *  **原文链接：** https://www.toutiao.com/item/6460323542046081550/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。