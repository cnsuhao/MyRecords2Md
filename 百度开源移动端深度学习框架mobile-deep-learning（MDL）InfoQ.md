---
title: 百度开源移动端深度学习框架mobile-deep-learning（MDL）
date: 2017-09-25 09:11:50
categories: "开发"
tags:
	- 移动互联网
	- 机器学习
	- 软件
	- GitHub
	- 深度学习

---

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL]

作者｜李永会

编辑｜Natalie

2017 年 9 月 25 日，百度在 GitHub 开源了移动端深度学习框架 mobile-deep-learning（MDL）的全部代码以及脚本，希望这个项目在社区的带动下能够更好地发展。

写在前面

深度学习技术已经在互联网的诸多方向产生影响，每天科技新闻中关于深度学习和神经网络的讨论越来越多。深度学习技术在近两年飞速发展，各种互联网产品都争相应用深度学习技术，产品对深度学习的引入也将进一步影响人们的生活。随着移动设备的广泛使用，在移动互联网产品应用深度学习和神经网络技术已经成为必然趋势。

与深度学习紧密联系在一起的图像技术同样在业界广泛应用。传统计算机视觉和深度学习的结合使图像技术得以快速发展。

GitHub 地址：

https://github.com/baidu/mobile-deep-learning

移动端深度学习技术应用

百度应用案例

在移动端应用深度学习技术比较典型的就是 CNN（Convolutional Neural Network）技术，即常被人提起的卷积神经网络。mobile-deep-learning（MDL）是一个基于卷积神经网络实现的移动端框架。

MDL 在移动端主要有哪些应用场景呢？比较常见的如分辨出一张图片中的物体是什么，也就是 **分类**；或者识别出一张图片中的物体在哪、有多大，也就是**主体识别**。

下面这个 App 叫拾相，可以在 Android 平台的应用宝中找到。它可以自动为用户将照片分类，对于拥有大量照片的用户来讲，这个功能很有吸引力。

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 1]

另外，在手机百度搜索框右侧可以打开图像搜索，打开图像搜索后的界面效果如下图。当用户在通用垂直类别下开启自动拍开关（图中下方标注）时，手停稳它就会自动找到物体进行框选，并无需拍照直接发起图像搜索。整个过程可以给用户带来流畅的体验，无需用户手动拍照。图片中的框体应用的就是典型的深度学习主体识别技术，使用的就是 mobile-deep-learning（MDL）框架。MDL 目前在手机百度中稳定运行了多个版本，经过数次迭代后可靠性大幅提升。

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 2]![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 3]

业界其他案例

互联网行业在移动端应用神经网络的案例已经越来越多。

目前的流派主要有两种，其一是完全在客户端运行神经网络，这种方式的优点显而易见，那就是不需要经过网络，如果能保证运行速度，用户体验会非常流畅。如果能保证移动端高效运行神经网络，可以使用户感觉不到加载过程。使用完全脱离互联网网络在移动端运算神经网络的 App 已经举例，如前述拾相和手机百度中的图像搜索。

其二是另一种，运算神经网络过程依赖互联网网络，客户端只负责 UI 展示。在客户端神经网络落地之前，绝大部分 App 都使用了这种运算在服务端、展示在客户端的方式。这种方式的优点是实现相对容易，开发成本更低。

为了更好理解上述两种神经网络的实现方法，下面展示两个识别植物花卉的例子，分别用到了识花和形色两个 App。这两款 App 都使用了典型分类方法，都可以在 iOS 平台的 App Store 中找到。下图是一张莲花图片，这张图片使用识花和形色两个 App 分类都能得到较好的分类结果。你可以尝试安装这两款 App 并根据使用效果来判断它们分别使用了上述哪一种方法。

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 4]

识花

近一年来涌现出很多花卉识别的 App。微软「识花」是微软亚洲研究院推出的一款用于识别花卉的 App，用户可以在拍摄后选择花卉，App 会给出该类花卉的相关信息。精准的花卉分类是其对外宣传的一大亮点。

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 5]

形色

这款「形色」App，只需要对准植物 (花、草、树) 拍照，就能快速给出植物的名字，还有不少有趣的植物知识，如这个植物还有什么别名、植物的花语、相关古诗词、植物文化、趣味故事以及养护方法，看完收获不少。

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 6]

移动端应用深度学习的难点

一直以来由于技术门槛和硬件条件的限制，在移动端应用深度学习的成功案例不多。传统移动端 UI 工程师在编写神经网络代码时，可以查阅到的移动端深度学习资料也很少。另一方面，时下的互联网竞争又颇为激烈，先入咸阳者王，可以率先将深度学习技术在移动端应用起来，就可以更早地把握时代先机。

移动端设备的运算能力相对 PC 端非常弱小。由于移动端的 CPU 要将功耗指标维持在很低的水平，制约了性能指标的提升。在 App 中做神经网络运算会使 CPU 运算量猛增。如何协调好用户功耗指标和性能指标就显得至关重要。

百度图像搜索客户端团队在 2015 年底就开始针对移动端深度学习技术应用进行攻关。最终，挑战性问题被逐一解决，现今相关代码已经在很多 App 上运行，这些 App 有日 PV 亿级的产品，也有创业期的产品。

在移动端应用深度学习技术本已困难重重，而在手机百度这种量级的产品上应用，更是要面对各种机型和硬件、手机百度的指标要求。如何使神经网络技术稳定高效运转是最大的考验。拆解问题就是移动端团队面对的首要问题。我们简单总结后发现移动端与服务器端进行对比更容易呈现问题和难点，继而在服务器端和客户端做了以下深度学习技术应用对比。

<table>
 <thead>
  <tr>
   <th>难点</th>
   <th>与服务器端对比</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>内存</td>
   <td>内存：服务器端弱限制 - 移动端内存有限</td>
  </tr>
  <tr>
   <td>耗电量</td>
   <td>耗电量：服务器端不限制 - 移动端严格限制</td>
  </tr>
  <tr>
   <td>依赖库体积</td>
   <td>依赖库体积：服务器端不限制 - 移动端强限制</td>
  </tr>
  <tr>
   <td>模型体积</td>
   <td>模型大小：服务器端常规模型体积 200M 起 - 移动端不宜超过 10M</td>
  </tr>
  <tr>
   <td>性能</td>
   <td>性能：服务器端强大 GPU BOX - 移动端 CPU 和 GPU</td>
  </tr>
 </tbody>
</table>

在开发过程中，团队逐步解决掉以上困难，形成了现在的 MDL 深度学习框架。为了让更多移动端工程师能够快速用轮子、专注业务，百度开源了全部相关代码，社区也欢迎任何人加入到造轮子的开发过程中来。

MDL 框架设计

设计思路

作为一款移动端深度学习框架，我们充分考虑到移动应用自身及运行环境的特点，在速度、体积、资源占用率等方面提出了严格的要求，因为其中任何一项指标对用户体验都有重大影响。

同时，可扩展性、鲁棒性、兼容性也是我们设计之初就考虑到了的。为了保证框架的可扩展性，我们对 layer 层进行了抽象，方便框架使用者根据模型的需要，自定义实现特定类型的层，我们期望 MDL 通过添加不同类型的层实现对更多网络模型的支持，而不需要改动其他位置的代码；为了保证框架的鲁棒性，MDL 通过反射机制，将 C++ 底层异常抛到应用层，应用层通过捕获异常对异常进行相应处理，如通过日志收集异常信息、保证软件可持续优化等；目前行业内各种深度学习训练框架种类繁多，而 MDL 不支持模型训练能力，为了保证框架的兼容性，我们提供 Caffe 模型转 MDL 的工具脚本，使用者通过一行命令就可以完成模型的转换及量化过程，后续我们会陆续支持 PaddlePaddle、TensorFlow 等模型转 MDL，兼容更多种类的模型。

总体架构

MDL 框架的总体架构设计图如下：

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 7]

MDL 框架主要包括模型转换模块（MDL Converter）、模型加载模块（Loader）、网络管理模块（Net）、矩阵运算模块（Gemmers）及供 Android 端调用的 JNI 接口层（JNI Interfaces）。其中，模型转换模块主要负责将 Caffe 模型转为 MDL 模型，同时支持将 32bit 浮点型参数量化为 8bit 参数，从而极大地压缩模型体积；模型加载模块主要完成模型的反量化及加载校验、网络注册等过程，网络管理模块主要负责网络中各层 Layer 的初始化及管理工作；MDL 提供了供 Android 端调用的 JNI 接口层，开发者可以通过调用 JNI 接口轻松完成加载及预测过程。

MDL 定位简单可用

MDL 开源项目在实施之初就已经有了清晰定位。在设备繁杂且性能较低的移动端平台技术研发过程中，能够为新颖的深度学习技术找到合适场景并应用到自己的产品中是非常吸引人的。但如果让每个移动端工程师在应用深度学习过程中都要重新写一次全部神经网络的实现，会增加较大成本。MDL 的定位是简单地使用和部署神经网络，如果使用基本功能则不需要进行过多配置和修改，甚至连机器学习库的编译过程都不需要，只需要关注具体业务实现、如何使用即可。

与此同时 MDL 简单清晰的代码结构也可以作为学习材料，为刚刚接触深度学习的研发工程师提供参考。因为我们在支持手机平台交叉编译同时，也支持 Linux 和 Mac 的 x86 平台编译，在调整深度学习代码的同时可以直接在工作电脑上编译运行，而不需要部署到 arm 平台。所需要的只是简单的几行代码，具体可以查阅 MDL 的 GitHub Readme。

    # https://github.com/baidu/mobile-deep-learning # mac or linux: ./build.sh mac cd build/release/x86/build ./mdlTest

复杂的编译过程往往比开发的时间更长，在 MDL 中只要一行./build.sh android 就能把 so、测试 test 都搞定，部署非常简便。

    ./build.sh android

MDL 的性能及兼容性

 *  体积 armv7 300k+
 *  速度 iOS GPU mobilenet 可以达到 40ms、squeezenet 可以达到 30ms

MDL 从立项到开源，已经迭代了一年多。移动端比较关注的多个指标都表现良好，如体积、功耗、速度。百度内部产品线在应用前也进行过多次对比，和已开源的相关项目对比，MDL 能够在保证速度和能耗的同时支持多种深度学习模型，如 mobilenet、googlenet v1、squeezenet 等，且具有 iOS GPU 版本，squeezenet 一次运行最快可以达到 3-40ms。

同类框架对比

<table>
 <thead>
  <tr>
   <th>框架</th>
   <th>Caffe2</th>
   <th>TensorFlow</th>
   <th>ncnn</th>
   <th>MDL(CPU)</th>
   <th>MDL(GPU)</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>硬件</td>
   <td>CPU</td>
   <td>CPU</td>
   <td>CPU</td>
   <td>CPU</td>
   <td>GPU</td>
  </tr>
  <tr>
   <td>速度</td>
   <td>慢</td>
   <td>慢</td>
   <td>快</td>
   <td>快</td>
   <td>极快</td>
  </tr>
  <tr>
   <td>体积</td>
   <td>大</td>
   <td>大</td>
   <td>小</td>
   <td>小</td>
   <td>小</td>
  </tr>
  <tr>
   <td>兼容</td>
   <td>Android&amp;amp;iOS</td>
   <td>Android&amp;amp;iOS</td>
   <td>Android&amp;amp;iOS</td>
   <td>Android&amp;amp;iOS</td>
   <td>iOS</td>
  </tr>
 </tbody>
</table>

与支持 CNN 的移动端框架对比，MDL 速度快、性能稳定、兼容性好、demo 完备。

兼容性

MDL 在 iOS 和 Android 平台均可以稳定运行，其中 iOS10 及以上平台有基于 GPU 运算的 API，性能表现非常出色，在 Android 平台则是纯 CPU 运行。高中低端机型运行状态和手机百度及其他 App 上的覆盖都有绝对优势。

MDL 同时也支持 Caffe 模型直接转换为 MDL 模型。

MDL 特性一览

在移动 AI 相关研发启动之初，百度图像搜索团队对比了大部分已经开源的同类 CNN 框架，百家争鸣的同时也暴露了该方向的问题。一些框架实验数据表现优秀，实际产品中或是表现较差且性能极不稳定，或是机型无法全覆盖，或是体积达不到上线标准。为了避免这些问题，MDL 加入了以下 Features：

 *  一键部署，脚本参数就可以切换 iOS 或者 Android
 *  支持 Caffe 模型全自动转换为 MDL 模型
 *  支持 GPU 运行
 *  已经测试过可以稳定运行 MobileNet、GoogLeNet v1、squeezenet 模型
 *  体积极小，无任何第三方依赖，纯手工打造
 *  提供量化脚本，直接支持 32 位 float 转 8 位 uint，模型体积量化后在 4M 上下
 *  与 ARM 相关算法团队线上线下多次沟通，针对 ARM 平台会持续优化
 *  NEON 使用涵盖了卷积、归一化、池化等运算过程
 *  loop unrolling 循环展开，为提升性能减少不必要的 CPU 消耗，全部展开判断操作
 *  将大量繁重的计算任务前置到 overhead 过程

后续规划

 *  为了让 MDL 体积进一步缩小，MDL 并未使用 protobuf 做为模型配置存储，而是使用了 Json 格式。目前 MDL 支持 Caffe 模型转换到 MDL 模型，未来会支持全部主流模型转换为 MDL 模型。
 *  随着移动端设备运算性能的提升，GPU 在未来移动端运算领域将会承担非常重要的角色，MDL 对于 GPU 的实现极为看重。目前 MDL 已经支持 iOS GPU 运行，iOS10 以上版本机型均可以使用。根据目前得到的统计数据显示，iOS10 已经涵盖绝大部分 iOS 系统，在 iOS10 以下可以使用 CPU 运算。除此之外，虽然 Android 平台目前的 GPU 运算能力与 CPU 相比整体偏弱，但日益涌现的新机型 GPU 已经越来越强大。MDL 后面也将加入 GPU 的 Feature 实现，基于 OpenCL 的 Android 平台 GPU 运算会让高端机型的运算性能再提升一个台阶。

欢迎开发者贡献代码

移动端神经网络的稳定高效运行，离不开诸多开发者的编码。MDL 长期本着靠谱运行、实用而不虚美的宗旨，希望能为移动端深度学习技术添砖加瓦。强烈欢迎有识之士济济加入，将深度学习技术在移动端广泛应用、播扬海内。

最后再次奉上 MDL 的 GitHub 目录：

https://github.com/baidu/mobile-deep-learning

##### 今日荐文 #####

点击下方图片即可阅读

![百度开源移动端深度学习框架mobile-deep-learning（MDL）][mobile-deep-learning_MDL 8]

百度正式开源其 RPC 框架 brpc


[mobile-deep-learning_MDL]: /pro/os/crawler/BZRQ-MFU2-Y6FV.jpg
[mobile-deep-learning_MDL 1]: /pro/os/crawler/Y6BA-YJZY-JMNJ.jpg
[mobile-deep-learning_MDL 2]: /pro/os/crawler/UBRU-BUAR-AUYF.jpg
[mobile-deep-learning_MDL 3]: /pro/os/crawler/IMJZ-YRUE-YJVJ.jpg
[mobile-deep-learning_MDL 4]: /pro/os/crawler/EBM6-BM3Q-JAVM.jpg
[mobile-deep-learning_MDL 5]: /pro/os/crawler/EIBI-FVIB-FRVQ.jpg
[mobile-deep-learning_MDL 6]: /pro/os/crawler/UF6V-VNQB-VURF.jpg
[mobile-deep-learning_MDL 7]: /pro/os/crawler/MIAV-Q3ZZ-AZVB.jpg
[mobile-deep-learning_MDL 8]: /pro/os/crawler/YQZN-UFQ3-IZVN.jpg
 *  **原文作者：** InfoQ
 *  **原文链接：** https://www.toutiao.com/item/6469517449988407821/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。