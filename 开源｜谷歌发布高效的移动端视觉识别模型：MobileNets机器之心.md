---
title: 开源｜谷歌发布高效的移动端视觉识别模型：MobileNets
date: 2017-06-15 11:50:07
categories: "开发"
tags:
	- Google
	- 机器学习
	- GitHub
	- 深度学习
	- 人工智能

---

选自Google Research

**机器之心编译**

**参与：蒋思源**

> 近日，谷歌开源了 MobileNets，一个支持多种视觉识别任务的轻量级模型，还能高效地在移动设备上运行。同时机器之心也关注过开源圈内利用苹果最新发布的 Core ML 实现的谷歌移动端神经网络 MobileNet。此外，谷歌的这次开源充分地体现了其「移动优先」与「AI 优先」的有机结合。

项目地址：https://github.com/tensorflow/models/blob/master/slim/nets/mobilenet\_v1.md

近年来，随着神经网络大大加强了视觉识别技术，深度学习令计算机视觉取得了极大的进展。虽然如今通过 Cloud Vision API 和联网设备提供了大量的计算机视觉应用，如目标识别、地标识别、商标和文本识别等，但我们相信随着移动设备的计算力日益增长，这些技术不论何时、何地、有没有联网都可以加载到用户的移动设备中。然而移动设备和嵌入式应用的视觉识别还存在着很多挑战，即模型必须在有限资源的环境中充分利用计算力、功率和储存空间以在高精度下快速运行。

因此近日谷歌发布了 MobileNets 网络架构，它是一系列在 TensorFlow 上高效、小尺寸的移动优先型视觉模型，其旨在充分利用移动设备和嵌入式应用的有限的资源，有效地最大化模型的准确性。MobileNets 是小型、低延迟、低功耗的参数化模型，它可以满足有限资源下的各种应用案例。它们可以像其他流行的大规模模型（如 Inception）一样用于分类、检测、嵌入和分割任务等。

![开源｜谷歌发布高效的移动端视觉识别模型：MobileNets][MobileNets]

应用案例包括目标检测、细粒度分类、人脸属性和地标识别等。

该版本可在 TensorFlow 中使用 TF-Slim 对 MobileNets 模型进行定义，同样还有 16 个预训练 ImageNet 分类保存点（checkpoints）以适用于所有大小的移动项目。这些模型可以借助 TensorFlow Mobile 在移动设备上高效地运行。

![开源｜谷歌发布高效的移动端视觉识别模型：MobileNets][MobileNets 1]

如上图所示，我们需要选择正确的 MobileNet 模型以符合所需的延迟和模型大小。内存和磁盘上的神经网络规模和参数的数量成正比。神经网络的延迟和功率大小与乘积累加（Multiply-Accumulates/MAC）数量成比例调整。MAC 度量了融合乘法和累加运算操作的数量。Top-1 和 Top-5 精度是在 ILSVRC 数据集上度量的。

如下图所示，MobileNets 权衡了模型的延迟、规模和准确度。

![开源｜谷歌发布高效的移动端视觉识别模型：MobileNets][MobileNets 2]

该版本可用 TF-Slim 对 MobileNets 模型进行定义。TF-slim 是用于定义、训练和评估复杂模型的 TensorFlow（tensorflow.contrib.slim）轻量级高层 API。其 Github 目录包含使用 TF-slim 训练和评估几种广泛使用的卷积神经网络（CNN）图像分类模型的代码，同时还包括脚本以允许从头开始训练模型或微调预训练模型。

谷歌表明他们很高兴能将 MobileNets 分享到开源社区中，读者也可以阅读以下资源进一步了解 MobileNets：

 *  使用该模型库的更多信息可以阅读 TensorFlow-Slim Image Classification Library ：https://github.com/tensorflow/models/blob/master/slim/README.md
 *  如何在移动设备上运行模型可以阅读 TensorFlow Mobile：https://www.tensorflow.org/mobile/

更详细的内容可阅读以下论文。

**论文：MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications**

论文链接：https://arxiv.org/abs/1704.04861v1

![开源｜谷歌发布高效的移动端视觉识别模型：MobileNets][MobileNets 3]

摘要：我们提出了 MobileNets：一种用于移动端和嵌入式视觉应用的新模型。它基于一种流线型架构，使用深度可分离卷积方法来构建轻量级深度神经网络。我们引入了两个简单的全局超参数，可以在延迟和准确性之间找到平衡点。这些超参数允许模型开发者针对应用面临的局限性选择正确尺寸的模型。在 ImageNet 分类任务中，我们的模型具有资源消耗和精度的平衡性，并展示了颇具竞争力的性能。我们也展示了 MobileNets 在多种不同应用中的有效性，其中包括物体检测、粒度分类、面部属性和大规模地理定位。


[MobileNets]: /pro/os/crawler/MZ67-V2I7-RBIV.jpg
[MobileNets 1]: /pro/os/crawler/FVNF-IQ3E-VF3E.jpg
[MobileNets 2]: /pro/os/crawler/QUEN-AQQU-AIJ2.jpg
[MobileNets 3]: /pro/os/crawler/IY3I-NNMR-2IYZ.jpg
 *  **原文作者：** 机器之心
 *  **原文链接：** https://www.toutiao.com/item/6431707544825102849/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。