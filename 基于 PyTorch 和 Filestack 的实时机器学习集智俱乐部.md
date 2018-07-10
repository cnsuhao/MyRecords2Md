---
title: 基于 PyTorch 和 Filestack 的实时机器学习
date: 2017-11-04 20:39:07
categories: "开发"
tags:
	- 机器学习
	- 图像处理
	- 深度学习
	- 人工智能
	- HTML

---

![基于 PyTorch 和 Filestack 的实时机器学习][PyTorch _ Filestack]

# **原题：** #

Realtime Machine Learning with PyTorch and Filestack

# **作者：** #

Richard Herbert

# **来源：** #

https://blog.filestack.com/tutorials/realtime-machine-learning-pytorch/

“深度学习”这个名词才出现没几年，就引领了整个数字时代。

过去的二十年中，随着像 Tensorflow，Torch 和 Theano 这样的主流学习框架的发展，使机器学习变得更加简单和高效。在以前前让人头疼的算法，现在已经变成解决大数据的分类和回归任务的标准。因此，**深度学习已经成为大数据公司最重要和基础的一部分。**

然而，深度学习需要有大量的标注数据，和极具代表性的案例。**Ian Goodfellow** 在他的“ 深度学习”（http://www.deeplearningbook.org/）一书中提到，至少要有 5000 个标注样本才能让模型得出一个**“可以接受”**的结果。

**对于没有明确市场定位的新型服务公司来说，这是很难实现的。**

一家公司必须收集到足够大量的带标签的案例，才能使用机器学习，这可能会在竞争时失去最好时机。同时他们也更需要花费大量的时间去训练模型，去调整对深度学习模型尤其重要的超参数。

幸运的是，最近在无监督学习和文件上传方面的改进意味着，实施和训练无标签或无监督的深度模型要比以前更加容易。**本文将展示如何使用 PyTorch、Filestack 和感知误差创建一个实时、无监督的深层自动编码器。**

**提示：本文假设读者对机器学习有基本的理解和经验**

# **获取数据** #

我们对训练数据的操作都需要 **Filestack** 来完成，所以我们需要编写一个简单的文件上传网页。

![基于 PyTorch 和 Filestack 的实时机器学习][PyTorch _ Filestack 1]

Filestack 提供了一个易于集成的文件上传器，用户可从主要云端存储提供商和本机中挑选文件。所以你现在需要免费注册https://dev.filestack.com/register/free，并获得一个 API 密钥将其嵌入到你的网页中。

    <head> <script src="https://static.filestackapi.com/v3/filestack-0.5.2.js"></script></head><body> <script> var client = filestack.init('YOUR_API_KEY_HERE') client.pick({ accept: ['image/jpeg', 'image/png'], fromSources: ['local_file_system', 'imagesearch', 'facebook', 'instagram', 'googledrive', 'dropbox', 'flickr', 'github', 'gmail', 'onedrive'] }) </script></body>

这是几行 HTML 和 Javascript 代码。通过这几行代码，你就可以拥有一个功能齐全的文件上传器，访问无数的云提供商，无需花费几个小时编写连接服务的代码。

从代码片段可以看出，Filestack 不限制开发人员指定文件类型或扩展名。因此，我们将把我们的输入限制为 JPEG 和 PNG；PIL 是 Python 的标准图片库，是适合卷积学习的最佳图像处理软件包。文件上传选择器支持许多预处理项，可让用户在上传之前裁剪和编辑照片，这点非常有用，比如可以用它来裁剪出人脸等等。

那么我们现在需要添加一些 Javascript 来发送图并等待服务器的响应（我们将发回自动编码的输出）。

    var pickFile = function(options){ client.pick(options).then(function(results) { var files = results.filesUploaded var fd = new FormData() fd.append('url', files[0].url) var xhr = new XMLHttpRequest() xhr.responseType = 'blob' xhr.addEventListener('load', function(response){ var data = response.target.response var image_field = document.getElementById('image') image_field.src = URL.createObjectURL(data) }) xhr.open('POST', 'http://localhost:5000') xhr.send(fd) })}

解决了文件上传的问题以后，我们可以着手建立一个深层的卷积网络了。

# **PyTorch** #

PyTorch 是一个相对较新的机器学习框架，基于 Python 并保留了 Torch 的可访问性和速度。它还提供了 Theano 和 Tensorflow 推广的使用计算图定义模型的机制，以及类似Torch 的序列风格定义方法。你可以在这里找到安装说明（**http://pytorch.org/**）。

![基于 PyTorch 和 Filestack 的实时机器学习][PyTorch _ Filestack 2]

如果你以前从未使用 PyTorch 或任何机器学习框架，请查看本教程（http://pytorch.org/tutorials/beginner/deep\_learning\_60min\_blitz.html），该教程将介绍基本操作和一些简单模型。还有一个专门针对 Torch 用户迁移 PyTorch 的教程（http://pytorch.org/tutorials/beginner/former\_torchies\_tutorial.html）。

# **自动编码** #

我们的模型将使用最新的基于感知误差技术的自动编码器，从图像流中获得重要视觉特征，这些都不需要标签。这种无监督的预训练从根本上减少了在分类和回归任务上收敛所需的样本量和训练时间。

传统的自编码器使用编码器-解码器模式：即使用编码器将图像（或其它数据）压缩至隐藏状态，再传入解码器转换成原始的格式。通过这种模式可以在无监督的情况下学习到图像的明显视觉特征。

传统自动编码器的问题是通过逐像素重建误差学习，通常使用均方误差回归。换句话说，他们在尝试学习无损压缩，有时会导致模糊，有时无法重建，表现不佳。近几年，这种降级的自动编码器只有初学者用来练习，而不适合深度学习工程师。

然而，使用感知误差可以从本质上提高自动编码器提取高可用特征的能力。

# **感知误差** #

**Prisma** 是一个能把你的照片变成艺术家作品的APP，主要是基于开源论文“**艺术风格的神经算法**”（https://arxiv.org/abs/1508.06576）。这篇论文中，作者创造了一种算法，通过使用预先训练的卷积网络 VGG19 转换图像，分别重构图像的内容和风格。作者发现，较高的卷积层保留了图像的内容，而较低的层似乎保留了其风格。如果你转换两个图像，你可以混合一个的内容与另一个的风格。

因此，只需要你手机里的一张自拍照，Prisma 就能把它变成一张类似于梵高的画作。**这种基于特征的提取的方法被称为感知误差，我们就基于这种方法训练我们的自动编码器。**

# **实现** #

我们希望构建传统的自动编码器，然后通过预训练网络传递其输出和原始图像，并对提取的特征计算损失，我们可以不让模型逐像素地精确重建图像，而是提取出图像中的特征。 我们要描述汤姆克鲁斯的大体形态：男性，瘦，棕色的头发，灿烂的笑容，你会用这些来描述他，而不是他的头发的确切的翻转，精确的背景，或他的西装的一些褶皱等等。

这样做意味着我们需要一个公共的， 训练好的网络，如 VGG，Inception 或者 Alexnet，而 PyTorch 自身内置了这些网络，所以使用起来极其方便！

# **实现感知损失模型** #

遵循论文中定义的通用算法，我们将使用 VGG19 深度卷积神经网络的 ReLU 层 “relu2\_2”。我们将自动编码器的输出以及原始训练图像传入神经网络，然后再计算误差。

    vgg = models.vgg19(pretrained=True)class VGG(nn.Module): def __init__(self): super(VGG, self).__init__() self.main = nn.Sequential( *list(vgg.features.children())[:9] ) def forward(self, x): return self.main(x)

重点是，我们不想在运行反向传播时改变 VGG 网络的梯度，所以我们需要检查每个 VGG 层，并添加一个允许 Autograd 的标志，将其设置为 False。这样 PyTorch 的计算图模块就知道不去更新那些层的梯度了。

    for param in vgg.parameters(): param.requires_grad = False

# **建立模型** #

本文没有提供卷积神经网络的深入教程。如果你想深入学习，我推荐 Ian Goodfellow 的深度学习 或 斯坦福大学深度学习课程（http://cs231n.stanford.edu/）的网站。

关于自动编码器，我使用了 Chintala 等人在其 DCGAN 网络中（https://arxiv.org/abs/1511.06434）采用的的反卷积架构，但是理论山来说，使用其它的编码器-解码器构架也是可以的。

不管你使用什么架构，我强烈建议一定要在网络中加入 “batch normalization” 和 使用 “Leaky ReLU” 作为激活函数。同时应该使用“tanh”作为解码器输出层的激活函数，而不是 sigmoid。

>     self.encoder = nn.Sequential( nn.Conv2d(channels, ndf, 4, 2, 1, bias=False), nn.LeakyReLU(0.2, inplace=True), nn.Conv2d(ndf, ndf * 2, 4, 2, 1, bias=False), nn.BatchNorm2d(ndf * 2), nn.LeakyReLU(0.2, inplace=True), nn.Conv2d(ndf * 2, ndf * 4, 4, 2, 1, bias=False), nn.BatchNorm2d(ndf * 4), nn.LeakyReLU(0.2, inplace=True), nn.Conv2d(ndf * 4, ndf * 8, 4, 2, 1, bias=False), nn.BatchNorm2d(ndf * 8), nn.LeakyReLU(0.2, inplace=True), nn.Conv2d(ndf * 8, ndf * 16, 4, 2, 1, bias=False), nn.BatchNorm2d(ndf * 16), nn.LeakyReLU(0.2, inplace=True), nn.Conv2d(ndf * 16, ndf * 32, 4, 2, 1, bias=False), nn.BatchNorm2d(ndf * 32), nn.LeakyReLU(0.2, inplace=True), nn.Conv2d(ndf * 32, z_dim, 4, 1, 0, bias=False),)

>     self.decoder = nn.Sequential( nn.ConvTranspose2d(z_dim, ngf * 32, 4, 1, 0, bias=False), nn.BatchNorm2d(ngf * 32), nn.LeakyReLU(0.2, inplace=True), nn.ConvTranspose2d(ngf * 32, ngf * 16, 4, 2, 1, bias=False), nn.BatchNorm2d(ngf * 16), nn.LeakyReLU(0.2, inplace=True), nn.ConvTranspose2d(ngf * 16, ngf * 8, 4, 2, 1, bias=False), nn.BatchNorm2d(ngf * 8), nn.LeakyReLU(0.2, inplace=True), nn.ConvTranspose2d(ngf * 8, ngf * 4, 4, 2, 1, bias=False), nn.BatchNorm2d(ngf * 4), nn.LeakyReLU(0.2, inplace=True), nn.ConvTranspose2d(ngf * 4, ngf * 2, 4, 2, 1, bias=False), nn.BatchNorm2d(ngf * 2), nn.LeakyReLU(0.2, inplace=True), nn.ConvTranspose2d(ngf * 2, ngf, 4, 2, 1, bias=False), nn.BatchNorm2d(ngf), nn.LeakyReLU(0.2, inplace=True), nn.ConvTranspose2d(ngf, channels, 4, 2, 1, bias=False), nn.Tanh())

对于优化算法，我使用 Adam 以保证学习速率，当然你可以选择其它算法。PyTorch 内几乎提供了所有流行的优化算法，但是如果你喜欢自定义算法，你可以通过 model.parameters 函数获得网络参数，自行进行优化。

    optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)

# **将Filestack整合到后端** #

Filestack 的免费版可以保存我们的图像并返回一个公共的 URL，然后我们可以把它发送到 Flask 服务器，在空闲的时候使用。这对于我们这种小实验来说已经非常完美了，如果你需要将文件存储在自己的S3存储器或其它存储空间的时候，你可随时升级。现在，我们需要将URL重定向到用户数据的最终位置，将URL发送到我们模型后台的深度学习服务器。

    def train_on_image(url): response = requests.get(url) image = Image.open(BytesIO(response.content)) image_tensor = loader(image).cuda() image_tensor = image_tensor.view(1, 3, 256, 256)

使用加载器的时候，我们可以将图像转换成张量并进行预处理，但我们需要使用 Torchvision，也就是 PyTorch 的图像转换库。

    loader = transforms.Compose([ transforms.Scale(256), transforms.CenterCrop(256), transforms.ToTensor()])

# **训练** #

等准备好图像张量，我们可以实时传递或者将他们组成一个小批次后再传递。

    model.zero_grad()target = Variable(image_tensor, volatile=True)output = model(Variable(image_tensor))

预训练的 PyTorch 模型期望对输入进行某种规范化处理，因此我们必须先使用这里声明的平均值和标准偏差来规范化输出，然后再传递给误差计算模型。需要注意的是这些更改必须通过 PyTorch 变量进行，因此可以将它们存储在计算图中。

    def normalize(x): """normalization for VGG input""" mean_tensor = torch.FloatTensor(x.data.size()).cuda() mean_tensor[:, 0, :, :] = 0.485 mean_tensor[:, 1, :, :] = 0.456 mean_tensor[:, 2, :, :] = 0.406 std_tensor = torch.FloatTensor(x.data.size()).cuda() std_tensor[:, 0, :, :] = 0.229 std_tensor[:, 1, :, :] = 0.224 std_tensor[:, 2, :, :] = 0.225 x = (x - Variable(mean_tensor)) / Variable(std_tensor)normalize(output)normalize(target)

然后，我们可以计算输出的感知特征的均方误差（或损耗函数），并进行反向传播以更新模型的梯度并进行优化。

    vgg_fake = vgg(output)vgg_real = vgg(target)loss = criterion(vgg_fake, Variable(vgg_real.data, requires_grad=False))loss.backward()g_optimizer.step()

# **结果** #

我从 CelebA 数据集中每次获取一张图片来训练模型，再使用另一套独立的数据集来测试模型。要注意的是，测试和训练集中含有大量的名人（尽管在不同的照片中）照片，而名人的照片往往会存在某些面部特征/年龄，这意味着该模型在非名人照片上表现不佳。

![基于 PyTorch 和 Filestack 的实时机器学习][PyTorch _ Filestack 3]

如图所示，这不是纯粹的（复制）重建图像，而是有特征、有风格地表征重建。因此我们可以很自信的说我们的实时、预训练的模型成功学习到了图片中明显的特征，这点可以应用在未来的诸如分类的各种任务中。

# **结论** #

总之，公司在实时训练模型时遇到的问题包括：使用高质量标签，和大量的连接云服务，而不依靠耗时间来开发。Filestack 使文件上传简单，流畅，可以快速部署数据服务，而使用自动编码器则可以满足对无监督学习的需求。使任何公司在大型标签数据集可用之前预先模拟其模型，也可能减少分类和回归任务模型收敛所需的样本量。

--------------------

# **推荐阅读：** #

[张江：“火炬上的深度学习”之缘起][Link 1]  


[未雨绸缪：随手保存 PyTorch 训练模型][PyTorch]  


[为什么他们要来集智AI学园学习 PyTorch？][AI_ PyTorch]  


[为什么机器学习研究者都投入了 PyTorch 的怀抱？][PyTorch 1]  


[深度学习新手必学：使用 Pytorch 搭建一个 N-Gram 模型][Pytorch _ N-Gram]  


--------------------

关注集智AI学园公众号

获取更多更有趣的AI教程吧！

搜索微信公众号：swarmAI

集智AI学园QQ群：426390994

学园网站：campus.swarma.org

商务合作｜zhangqian@swarma.org

投稿转载｜wangjiannan@swarma.org


[PyTorch _ Filestack]: /pro/os/crawler/JFAM-EUVZ-REIZ.jpg
[PyTorch _ Filestack 1]: /pro/os/crawler/ZFNI-3UQ6-RBM3.jpg
[PyTorch _ Filestack 2]: /pro/os/crawler/EZJV-UMAF-IJRE.jpg
[PyTorch _ Filestack 3]: /pro/os/crawler/JUYJ-ERFF-IR6R.gif
[Link 1]: https://www.toutiao.com/i6483303556739760654/
[PyTorch]: https://www.toutiao.com/i6483759339109614093/
[AI_ PyTorch]: https://www.toutiao.com/i6480109739299586574/
[PyTorch 1]: https://www.toutiao.com/i6478979807391515150/
[Pytorch _ N-Gram]: https://www.toutiao.com/i6481414294016623117/
 *  **原文作者：** 集智俱乐部
 *  **原文链接：** https://www.toutiao.com/item/6484537958425690637/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。