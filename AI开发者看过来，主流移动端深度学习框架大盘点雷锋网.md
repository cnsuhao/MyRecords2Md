---
title: AI开发者看过来，主流移动端深度学习框架大盘点
date: 2018-05-10 09:57:47
categories: "开发"
tags:
	- 机器学习
	- 软件
	- 深度学习
	- 编程语言
	- 人工智能

---

> **雷锋网按：**移动设备相较于 PC ，携带便携，普及率高。近年来，随着移动设备的广泛普及与应用，在移动设备上使用深度学习技术的需求开始涌现。

简书作者 dangbo 在《移动端深度学习展望》一文中对现阶段的移动端深度学习做了相关展望。作者认为，现阶段的移动端 APP 主要通过以下两种模式来使用深度学习：

 *  online 方式：移动端做初步预处理，把数据传到服务器执行深度学习模型，优点是这个方式部署相对简单，将现成的框架（Caffe，Theano，MXNet，Torch) 做下封装就可以直接拿来用，服务器性能大, 能够处理比较大的模型，缺点是必须联网。
 *  offline 方式：在服务器上进行训练的过程，在手机上进行预测的过程。

![AI开发者看过来，主流移动端深度学习框架大盘点][AI]

当前移动端的三大框架（Caffe2、TensorFlow Lite、Core ML）均使用 offline 方式，该方式可在无需网络连接的情况下确保用户数据的私密性。

各主流移动端深度学习框架诞生时间如下：

 *  2017 年 3 月，XMART LABS 在 GitHub 上开源 Bender
 *  2017 年 4 月 19 日，Facebook 在 F8 开发者大会上推出 Caffe2
 *  2017 年 5 月 17 日，在 Google I/O 2017 大会上，移动端深度学习框架 TensorFlow Lite 诞生
 *  2017 年 6 月 6 日，苹果在 WWDC 大会上推出 Core ML
 *  2017 年 9 月 25 日，百度开源移动端深度学习框架 mobile-deep-learning（MDL）
 *  ......

接下来，雷锋网 AI 研习社将介绍当前主流的移动端深度学习框架，其中包括移动端三大框架——Facebook、谷歌、苹果三大巨头发布的 Caffe2、TensorFlow Lite、Core ML，新秀 Bender，国产百度 MDL 以及支持移动端的 MXNet，以便刚刚入坑的开发者们对这些框架有初步的了解和认识。

### **Facebook 开源 Caffe2，最终将其并入 PyTorch** ###

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 1]

2017 年 4 月 19 日的 F8 年度开发者大会上，Facebook 发布了一款全新的开源深度学习框架——Caffe2，按照 Caffe2 官网介绍，它最大的特点就是轻量、模块化和可扩展性，即一次编码，到处运行（和 Java 的宣传语类似）。说得更直白一点，就是 Caffe2 可以方便地为手机等移动终端设备带来 AI 加持，让 AI 从云端走向终端。

Caffe2 在此前流行的开源框架 Caffe 基础上进行了重构和升级，一方面集成了诸多新出现的算法和模型，另一方面在保证运算性能和可扩展性的基础上，重点加强了框架在轻量级硬件平台的部署能力。它可以部署在包括 iOS，Android，英伟达 Tegra X1 和树莓派（Raspberry Pi）等在内的各种移动平台上。用户只需要加载 Caffe2 框架，然后通过几行简单的 API 接口调用（Python 或 C++），就能在手机 APP 上实现图像识别、自然语言处理和计算机视觉等各种 AI 功能。此外，Caffe2 还对 Caffe 平台的另一项核心竞争力：Model Zoo 社区方面提供了完整的支持。

除了框架自身，Caffe2 还获得了一系列的云平台支持，例如亚马逊 AWS 旗下的 Deep Learning AMI和微软 Azure 旗下 Data Science Virtual Machine (DSVM)，另外也获得了英伟达和高通的硬件平台支持。目前，Caffe2 框架已经被 Facebook 内部采用，开发者和研究人员们正在使用该框架提供的各种工具训练大型的机器学习模型，并为 Facebook 旗下的移动应用提供 AI 智能体验。

现在 Caffe2 代码也已正式并入 PyTorch，来使 Facebook 能在大规模服务器和移动端部署时更流畅地进行 AI 研究、训练和推理。

Caffe2 官网：http://Caffe2.ai/

GitHub 地址：https://github.com/Caffe2/Caffe2

Caffe2go 移动端项目 Github 链接: https://github.com/Caffe2/Caffe2

### **谷歌移动端深度学习框架 TensorFlow Lite，有望成为移动端模型部署推荐解决方案** ###

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 2]

谷歌于美国时间 2017 年 11 月 14 日正式发布 TensorFlow Lite 预览版，这一框架主要用于移动端和嵌入式设备，顾名思义，相较于 TensorFlow，TensorFlow Lite 是一个轻量化版本。这个开发框架专门为机器学习模型的低延迟推理做了优化，专注于更少的内存占用以及更快的运行速度。

TensorFlow Lite 具备以下三个重要功能：

轻量级（Lightweight）：支持机器学习模型的推理在较小二进制数下进行，能快速初始化/启动

跨平台（Cross-platform）：可以在许多不同的平台上运行，现在支持 Android 和 iOS

快速（Fast）：针对移动设备进行了优化，包括大大减少了模型加载时间、支持硬件加速

**结构**

下图是 TensorFlow Lite 的结构设计：

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 3]

模块如下:

 *  TensorFlow Model: 存储在硬盘上已经训练好的 TensorFlow 模型
 *  TensorFlow Lite Converter: 将模型转换为 TensorFlow Lite 文件格式的程序
 *  TensorFlow Lite Model File: 基于 FlatBuffers 的模型文件格式，针对速度和大小进行了优化。

**模型**

TensorFlow Lite 目前支持很多针对移动端训练和优化好的模型。

 *  MobileNet：能够识别 1000 种不同对象类的视觉模型，为实现移动和嵌入式设备的高效执行而设计。
 *  Inception v3：图像识别模型，功能与 MobileNet 相似，它提供更高的精度，但相对来说更大。
 *  Smart Reply： 设备对话模型，可以即时回复聊天消息，在 Android Wear 上有使用这一功能。
 *  Inception v3 和 MobileNets 已经在 ImageNet 数据集上训练了。大家可以利用迁移学习来轻松地对自己的图像数据集进行再训练。

TensorFlow Lite 发布一个月后，谷歌即宣布与苹果达成合作——TensorFlow Lite 将支持 Core ML。TensorFlow Lite 为 Core ML 提供支持后，iOS 开发者就可以利用 Core ML 的优势来部署模型。

目前，该框架还在不断更新与升级中，随着 TensorFlow 的用户群体越来越多，同时得益于谷歌的背书，假以时日，TensorFlow Lite 极大可能会成为在移动端和嵌入式设备上部署模型的推荐解决方案。

TensorFlow Lite 文档页面：http://Tensorflow.org/mobile/tflite

Core ML 转化器页面：https://github.com/tf-coreml/tf-coreml

pypi pip 安装包地址：https://pypi.python.org/pypi/tfcoreml/0.1.0

### **苹果 Core ML：离线状态下，隐私与 AI 可兼得** ###

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 4]

苹果在 2017WWDC 大会更新 iOS 11 时一并推出了面向开发者的全新机器学习框架——Core ML，声称能让本地数据处理愈加方便快捷。据介绍，Core ML 提供支持人脸追踪、人脸检测、地标、文本检测、条码识别、物体追踪、图像匹配等任务的 API。

据雷锋网 AI 研习社了解，Core ML 是一个基础机器学习框架，能用于众多苹果的产品，包括 Siri、相机和 QuickType。据官方介绍，Core ML 带来了极速的性能提升和机器学习模型的轻松整合，能将众多机器学习模型集成到 APP 中。它不但有 30 多种层来支持广泛的深度学习，而且还支持诸如树集成、SVM 和广义线性模型等标准模型。

苹果在 Core ML 开发文档中如此介绍：

> 使用 Core ML，你可以将训练好的模型整合进自己开发的 APP 中。Core ML 支持用于图像分析的 Vision、用于自然语言处理的 Foundation（如 NSLinguisticTagger 类）和用于评估已经学习到的决策树的 GameplayKit。Core ML 构建在 Accelerate、BNNS 和 Metal Performance Shaders 之上。

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 5]

Core ML 在设备上严格运行，确保了用户隐私数据，在无网络连接的情况下依然能够响应用户操作。

**CORE ML 相关技术**

 *  Metal 是针对 iPhone 和 iPad 中 GPU 编程的高度优化的框架，Metal 相较 OpenGL ES 能耗显著降低。另外，Metal 可以预估 GPU 状态来避免多余的验证和编译
 *  Metal Performance Shader 是苹果推出的一套借助 Metal 在 iOS 上实现深度学习的工具，它主要封装了 MPSImage 来存储数据管理内存，实现了 Convolution、Pooling、Fullconnetcion、ReLU 等常用的卷积神经网络中的 Layer

可以在 iPhone 内置应用中利用 Core ML 的优势，提升或实现如 Siri 语音识别、相机应用中识别人脸、QuickType 打字联想等新特性。Core ML+Vision 应用场景如下所示：

 *  在相机或给定图像中检测人脸
 *  检测眼睛和嘴巴的位置、头部形状等人脸面部详细特征
 *  录制视频过程中追踪移动的对象和确定地平线的角度
 *  转换两个图像，使其内容对齐，识别图像中的文本
 *  检测和识别条形码
 *  ......

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 6]

另外，还可以使用 Vision 驱动 Core ML，即在使用 Core ML 进行机器学习时，用 Vision 框架进行一些数据预处理。

Core ML 文档地址：https://developer.apple.com/documentation/coreml

### **Bender：基于 Metal 的机器学习框架** ###

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 7]

2017 年 3 月份左右，XMART LABS 在 GitHub 上开源了 Bender，它是一个基于 Metal 的机器学习框架，它允许你在 IOS APP 上轻松地定义和运行神经网络，该框架在底层使用了苹果的 Metal Performance Shaders。

XMART LABS 在 Github 上对 Bender 进行了这样的描述：

> Bender 是 MetalPerformanceShaders 之上的一个可用来操作神经网络的抽象层（abstraction layer）的工具。Bender 允许你使用卷积、池化、全连接以及一些规范化等最常见的 layer 来轻松地定义和运行神经网络。XMART LABS 还想加载在其他框架（TensorFlow 或者 Caffe2 等框架）上训练好的模型，现在的 Bender 已经内置了一个 TensorFlow 适配器（其可加载带有变量的图，并将其「翻译」成 Bender 的 layer），并计划将其功能大大增强。

**优势**

 *  Bender 支持选择 Tensorflow、 Keras、Caffe 等框架来运行已训练的模型，无论是在将训练好的模型 freeze，还是将权重导至 files（官方表示该支持特性即将到来）
 *  可直接从支持的平台导入一个 frozen graph 或者重新定义神经网络结构并加载权重。这两项操作均只需花费几分钟
 *  Bender 支持最常用的机器学习节点和 layer，同时其也具有可扩展性，因而你可以编写自己的可定义函数

使用 Bender 开发 APP 的示例如下。

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 8]

**Bender 有何优势？**

 *  Bender 能解决 MetalPerformanceShaders（iOS 中可使用的框架）中对开发者不太友好导致需要大量重复代码的问题
 *  TensorFlow 虽然可为 iOS 进行编译，但它并不支持在 GPU 上运行，而 Bender 的适配器则可以将 TF graph 解析并翻译成 Bender layer

Bender 页面网址：https://xmartlabs.github.io/Bender/

Github 地址：https://github.com/xmartlabs/Bender

### **百度开源移动端深度学习框架 MDL，可在苹果安卓系统自由切换** ###

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 9]

2017 年 9 月，百度在 GitHub 上开源了移动端深度学习框架 mobile-deep-learning（MDL）的全部代码以及脚本，这项研究旨在让卷积神经网络（CNNC）能更简单和高速的部署在移动端，支持 iOS GPU，目前已经在百度 APP 上有所使用。

MDL 具有体积小和速度快的特点。

 *  大小：340k+（在 arm v7 上）
 *  速度：对于 iOS Metal GPU Mobilenet，速度是 40ms，对于 Squeezenet，速度是 30ms

**特征**

 *  一键部署，可以通过修改参数在 iOS 和 Android 端之间转换
 *  iOS GPU 上支持运行 MobileNet 和 Squeezenet 模型
 *  在 MobileNet、GoogLeNet v1 和 Squeezenet 模型下都很稳定
 *  占用空间极小（4M），不需要依赖第三方的库
 *  支持从 32 比特 float 到 8 比特 unit 转化
 *  接下来会与与 ARM 相关的算法团队进行线上线下沟通，优化 ARM 平台
 *  NEON 使用涵盖了所有的卷积、归一化、池化等
 *  利用循环展开，可以让性能更加优化，防止不必要的 CPU 损失
 *  对于 overhead 进程，可以转发大量繁重的计算任务

GitHub 地址：https://github.com/baidu/mobile-deep-learning

### **MXNet： AWS 官方深度学习框架，支持移动端开发** ###

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 10]

MXNet 是一款开源的、轻量级、可移植的、灵活的深度学习库，它让用户可以混合使用符号编程模式和指令式编程模式来最大化效率和灵活性，目前已经是 AWS 官方推荐的深度学习框架。

MXNet 的核心是一个动态的依赖调度器，支持自动将计算任务并行化到多个 GPU 或分布式集群（支持 AWS、Azure、Yarn 等)。它上层的计算图优化算法可以让符号计算执行得非常快，而且节约内存，开启 mirror 模式会更加省内存，甚至可以在某些小内存 GPU 上训练其他框架因显存不够而训练不了的深度学习模型。

MXNet 支持在移动设备（Android、iOS）上运行基于深度学习的图像识别等任务，它的性能如下：

 *  依赖少，内存要求少，对于 Android 性能变化大的手机，通用性更高
 *  MXNet 需要先使用 ndk 交叉编译项目中的 amalgamation，可以根据自己的需求，修改 jni 中的接口，然后，编译好的动态链接库替换掉 Android demo 中的
 *  MXNet 提供了对 Caffe 模型的支持，通过提供的工具将 Caffe 训练好的模型进行转化 json 格式，随后在移动端使用

此外，MXNet 的一个很大的优点是支持多语言封装，比如 C++、Python、R、Julia、Scala、Go、MATLAB 和 JavaScript。在 MXNet 中构建一个网络需要的时间可能比 Keras、Torch 这类高度封装的框架要长，但是比直接用 Theano 等要快。MXNet 的各级系统架构（下面为硬件及操作系统底层，逐层向上为越来越抽象的接口）如下图所示。

![AI开发者看过来，主流移动端深度学习框架大盘点][AI 11]

GitHub：http://github.com/dmlc/MXNet

Apache MXNet 官方网站：https://MXNet.incubator.apache.org/

（完）


[AI]: /pro/os/crawler/JEUJ-ZVEU-IFV2.jpg
[AI 1]: /pro/os/crawler/NVFA-6NVJ-FMF2.jpg
[AI 2]: /pro/os/crawler/UYZN-6JVZ-NAZ3.jpg
[AI 3]: /pro/os/crawler/BA2Q-FREU-3EIA.jpg
[AI 4]: /pro/os/crawler/JVZZ-UVUN-IZAN.jpg
[AI 5]: /pro/os/crawler/6JZB-RFJV-2QRR.jpg
[AI 6]: /pro/os/crawler/R6JJ-FVFI-QAI3.jpg
[AI 7]: /pro/os/crawler/MFQN-YRFM-MZRY.jpg
[AI 8]: /pro/os/crawler/F7ZU-QM2I-JNMN.jpg
[AI 9]: /pro/os/crawler/UFV6-VQNM-6NFM.gif
[AI 10]: /pro/os/crawler/IJEU-YAMV-JFQB.jpg
[AI 11]: /pro/os/crawler/BBNE-QYMJ-AZZN.jpg
 *  **原文作者：** 雷锋网
 *  **原文链接：** https://www.toutiao.com/item/6553765619840320013/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。