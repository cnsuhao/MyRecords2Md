---
title: Tools for AI：以开发者为中心的全平台人工智能开发套件
date: 2018-05-21 23:09:12
categories: "开发"
tags:
	- Azure
	- 机器学习
	- 编程语言
	- 微软
	- 云计算

---

![Tools for AI：以开发者为中心的全平台人工智能开发套件][Tools for AI]

在前不久的微软Build 2018大会上，微软围绕“如何利用人工智能技术来构建更智能的云和边缘计算”这个中心，介绍了从Cognitive Services、Bot Framework、 Azure ML到Brainwave等一系列产品更新。而在今天召开的2018微软人工智能大会（Microsoft AI Innovate 2018）上，微软又针对中国市场推出一系列新技术和工具，包括由中国研发团队主导开发的Tools for AI人工智能开发套件。

Tools for AI为开发者提供了一个**全平台**、**全软件产品生命周期**、**支持各种深度学习框架的开发套件**。开发者可以通过熟悉的Visual Studio和Visual Studio Code开发工具，快速���发深度学习相关的程序。Tools for AI的**一键安装**功能可以帮助开发者配置深度学习的开发环境，配合Visual Studio (Code)自带的Python语言开发功能，开发者可以方便地编辑和调试基于CNTK、TensorFlow、PyTorch等主流深度学习框架下构建的深度学习训练程序。

![Tools for AI：以开发者为中心的全平台人工智能开发套件][Tools for AI 1]

开发者不仅可以方便地在本地编辑和调试训练程序，还可���**充分利用云端的计算资源加速**训练。针对不同开发者所拥有的云端资源差异，Tools for AI提供了多种支持。对于已拥有小规模自有训练资源的开发者，Tools for AI支持任意远端的Linux服务器或者基于容器的服务器；对于已经采用Azure ML、Azure Batch for AI等云端高级训练服务的客户，Tools for AI也可直接支持；对于想自己搭建较大规模训练集群的开发者，Tools for AI则通过与开源开放深���学习平台软件Open Platform for AI (OpenPAI) 合作提供支持。

值得一提的是，对于云端训练，Tools for AI可通过**统一的可视化界面**对训练任务、数据进行管理。可视化的调试工具、参数自动选择工具等高级功能的继承，也将帮助开发者更加高效地利用云端训练资源。

![Tools for AI：以开发者为中心的全平台人工智能开发套件][Tools for AI 2]

从模型到应用，一直是深度学习技术落地的关键一环。基于深度学习模型标准ONNX和微软最近发布的SDK WinML，Tools for AI可以帮助用户开发基于Universal Windows Platform (UWP)的应用程序。通过自带的模型转换工具和运行库，Tools for AI也能帮助用户开发Android、iOS上的应用。此外，基于Tools for AI，开发者还可以利用Cognitive Services微软认知服务等预先开发好的深度学习模型和服务来开发应用程序。

Tools for AI开发套件的特性可以总结为��

1. **Tools for AI与Visual Studio (Code) 配合****，为开发者提供了一个快速入门深度学习开发的集成开发环境**，包括：

 *  跨平台的Python编辑调试环境

 *  一键安装所有主流深度学习框架开发环境，包括CNTK、TensorFlow、PyTorch、Caffe2、MXNet等

 *  包含庞大的样例库和项目模板等

2. **Tools for AI可与各层级云端紧密集成，方便开发者利用云端资源管理和训练深度学习模型**，支持的云端服���包括：

 *  任意的远程Linux服务器

 *  基于容器技术的服务器

 *  Azure上的DLVM虚拟机；Azure ML服务；Azure Batch AI服务

 *  深度学习平台软件Open Platform for AI (OpenPAI)

3. **Tools for AI提供完整的人工智能开发生命周期管理功能**，包括模型训练、模型转换、应用程序开发等。

4. **Tools for AI和微软预建的高阶人工智能服务（例如微软认知服务Cognitive Services）相结合**，可帮助开发者更快地开发应用程序。

秉持开放、以开发者为中心的设计理念，Tools for AI致力于提供给开发者一个熟悉、一致和开放的开发环境，帮助他们完成深度学习开发全生命周期的所有工作。了解更多关于Tools for AI人工智能开发套件的信息，欢迎访问：

https://www.visualstudio.com/downloads/ai-tools-vs/

https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai

想了解微软人工智能大会的更多内容？请查看公众号的第二条推送。

**你也许还想****看****：**

![Tools for AI：以开发者为中心的全平台人工智能开发套件][Tools for AI 3]

感谢你关注“微软研究院AI头条”，我们期待你的留言和投稿，共建交流平台。来稿请寄：msraai@microsoft.com。


[Tools for AI]: /pro/os/crawler/ZF3A-MI2A-Q22M.gif
[Tools for AI 1]: /pro/os/crawler/QJE6-3EF6-3UBI.jpg
[Tools for AI 2]: /pro/os/crawler/MRMJ-RUVV-EF22.jpg
[Tools for AI 3]: /pro/os/crawler/IBFM-RUMM-6JBR.jpg
 *  **原文作者：** 微软亚洲研究院
 *  **原文链接：** https://www.toutiao.com/item/6558051500646466055/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。