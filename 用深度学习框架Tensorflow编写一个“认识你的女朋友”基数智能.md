---
title: 用深度学习框架Tensorflow编写一个“认识你的女朋友”
date: 2017-11-02 14:35:31
categories: "开发"
tags:
	- 摄影
	- 机器学习
	- Linux
	- OpenCV
	- 深度学习

---

大家都知道TensorFlow是Google旗下的深度学习技术框架，可被用于语音识别或图像识别等多项机器深度学习领域，它可在小到一部智能手机、大到数千台数据中心服务器的各种设备上运行。而且TensorFlow将完全开源，任何人都可以用，本文内容版权来自技术博主Tumumu，以下是内容正文：

想要她认得我，就需要给她一些我的照片，让她记住我的人脸特征，为了让她区分我和其他人，还需要给她一些其他人的照片做参照，所以就需要两组数据集来让她学习，如果想让她多认识几个人，那多给她几组图片集学习就可以了。下面就开始让我们来搭建这个能认识我的"她"。

![用深度学习框架Tensorflow编写一个“认识你的女朋友”][Tensorflow]

1、运行环境

系统：window或linux

软件：python 3.x 、 tensorflow

python支持库：Tensorflow

> pip install tensorflow \#cpu版本
> 
> pip install rensorflow-gpu \#gpu版本（需要cuda与cudnn的支持，不清楚的可以选择cpu版）
> 
> numpy:
> 
> pip install numpy
> 
> opencv:
> 
> pip install opencv-python
> 
> dlib:
> 
> pip install dlib

2、获取本人图片集

获取本人照片的方式当然是拍照了，我们需要通过程序来给自己拍照，如果你自己有照片，也可以用那些现成的照片，但前提是你的照片足够多。这次用到的照片数是10000张，程序运行后，得坐在电脑面前不停得给自己的脸摆各种姿势，这样可以提高训练后识别自己的成功率，在程序中加入了随机改变对比度与亮度的模块，也是为了提高照片样本的多样性。

程序中使用的是dlib来识别人脸部分，也可以使用opencv来识别人脸，在实际使用过程中，dlib的识别效果比opencv的好，但opencv识别的速度会快很多，获取10000张人脸照片的情况下，dlib大约花费了1小时，而opencv的花费时间大概只有20分钟。opencv可能会识别一些奇怪的部分，所以综合考虑之后我使用了dlib来识别人脸。

> get\_my\_faces.py
> 
> import cv2
> 
> import dlib
> 
> import os
> 
> import sys
> 
> import random
> 
> output\_dir = './my\_faces'
> 
> size = 64
> 
> if not os.path.exists(output\_dir):
> 
> os.makedirs(output\_dir)
> 
> \# 改变图片的亮度与对比度
> 
> def relight(img, light=1, bias=0):
> 
> w = img.shape\[1\]
> 
> h = img.shape\[0\]
> 
> \#image = \[\]
> 
> for i in range(0,w):
> 
> for j in range(0,h):
> 
> for c in range(3):
> 
> tmp = int(img\[j,i,c\]\*light + bias)
> 
> if tmp > 255:
> 
> tmp = 255
> 
> elif tmp < 0:
> 
> tmp = 0
> 
> img\[j,i,c\] = tmp
> 
> return img
> 
> \#使用dlib自带的frontal\_face\_detector作为我们的特征提取器
> 
> detector = dlib.get\_frontal\_face\_detector()
> 
> \# 打开摄像头 参数为输入流，可以为摄像头或视频文件
> 
> camera = cv2.VideoCapture(0)
> 
> index = 1
> 
> while True:
> 
> if (index <= 10000):
> 
> print('Being processed picture %s' % index)
> 
> \# 从摄像头读取照片
> 
> success, img = camera.read()
> 
> \# 转为灰度图片
> 
> gray\_img = cv2.cvtColor(img, cv2.COLOR\_BGR2GRAY)
> 
> \# 使用detector进行人脸检测
> 
> dets = detector(gray\_img, 1)
> 
> for i, d in enumerate(dets):
> 
> x1 = d.top() if d.top() > 0 else 0
> 
> y1 = d.bottom() if d.bottom() > 0 else 0
> 
> x2 = d.left() if d.left() > 0 else 0
> 
> y2 = d.right() if d.right() > 0 else 0
> 
> face = img\[x1:y1,x2:y2\]
> 
> \# 调整图片对比度与亮度， 对比度与亮度值取随机数，这样能增加样本多样性
> 
> face = relight(face, random.uniform(0.5, 1.5), random.randint(-50, 50))
> 
> face = cv2.resize(face, (size,size))
> 
> cv2.imshow('image', face)
> 
> cv2.imwrite(output\_dir+'/'+str(index)+'.jpg', face)
> 
> index += 1
> 
> key = cv2.waitKey(30) & 0xff
> 
> if key == 27:
> 
> break
> 
> else:
> 
> print('Finished!')
> 
> break

在这里我也给出一个opencv来识别人脸的代码示例：

> import cv2
> 
> import os
> 
> import sys
> 
> import random
> 
> out\_dir = './my\_faces'
> 
> if not os.path.exists(out\_dir):
> 
> os.makedirs(out\_dir)
> 
> \# 改变亮度与对比度
> 
> def relight(img, alpha=1, bias=0):
> 
> w = img.shape\[1\]
> 
> h = img.shape\[0\]
> 
> \#image = \[\]
> 
> for i in range(0,w):
> 
> for j in range(0,h):
> 
> for c in range(3):
> 
> tmp = int(img\[j,i,c\]\*alpha + bias)
> 
> if tmp > 255:
> 
> tmp = 255
> 
> elif tmp < 0:
> 
> tmp = 0
> 
> img\[j,i,c\] = tmp
> 
> return img
> 
> \# 获取分类器
> 
> haar = cv2.CascadeClassifier('haarcascade\_frontalface\_default.xml')
> 
> \# 打开摄像头 参数为输入流，可以为摄像头或视频文件
> 
> camera = cv2.VideoCapture(0)
> 
> n = 1
> 
> while 1:
> 
> if (n <= 10000):
> 
> print('It\`s processing %s image.' % n)
> 
> \# 读帧
> 
> success, img = camera.read()
> 
> gray\_img = cv2.cvtColor(img, cv2.COLOR\_BGR2GRAY)
> 
> faces = haar.detectMultiScale(gray\_img, 1.3, 5)
> 
> for f\_x, f\_y, f\_w, f\_h in faces:
> 
> face = img\[f\_y:f\_y+f\_h, f\_x:f\_x+f\_w\]
> 
> face = cv2.resize(face, (64,64))
> 
> '''
> 
> if n % 3 == 1:
> 
> face = relight(face, 1, 50)
> 
> elif n % 3 == 2:
> 
> face = relight(face, 0.5, 0)
> 
> '''
> 
> face = relight(face, random.uniform(0.5, 1.5), random.randint(-50, 50))
> 
> cv2.imshow('img', face)
> 
> cv2.imwrite(out\_dir+'/'+str(n)+'.jpg', face)
> 
> n+=1
> 
> key = cv2.waitKey(30) & 0xff
> 
> if key == 27:
> 
> break
> 
> else:
> 
> break

3、获取其他人脸图片集

需要收集一个其他人脸的图片集，只要不是自己的人脸都可以，可以在网上找到，先将下载的图片集，解压到项目目录下的input\_img目录下，也可以自己指定目录（修改代码中的input\_dir变量），接下来使用dlib来批量识别图片中的人脸部分，并保存到指定目录下

> set\_other\_people.py
> 
> \# -\*- codeing: utf-8 -\*-
> 
> import sys
> 
> import os
> 
> import cv2
> 
> import dlib
> 
> input\_dir = './input\_img'
> 
> output\_dir = './other\_faces'
> 
> size = 64
> 
> if not os.path.exists(output\_dir):
> 
> os.makedirs(output\_dir)
> 
> \#使用dlib自带的frontal\_face\_detector作为我们的特征提取器
> 
> detector = dlib.get\_frontal\_face\_detector()
> 
> index = 1
> 
> for (path, dirnames, filenames) in os.walk(input\_dir):
> 
> for filename in filenames:
> 
> if filename.endswith('.jpg'):
> 
> print('Being processed picture %s' % index)
> 
> img\_path = path+'/'+filename
> 
> \# 从文件读取图片
> 
> img = cv2.imread(img\_path)
> 
> \# 转为灰度图片
> 
> gray\_img = cv2.cvtColor(img, cv2.COLOR\_BGR2GRAY)
> 
> \# 使用detector进行人脸检测 dets为返回的结果
> 
> dets = detector(gray\_img, 1)
> 
> \#使用enumerate 函数遍历序列中的元素以及它们的下标
> 
> \#下标i即为人脸序号
> 
> \#left：人脸左边距离图片左边界的距离 ；right：人脸右边距离图片左边界的距离
> 
> \#top：人脸上边距离图片上边界的距离 ；bottom：人脸下边距离图片上边界的距离
> 
> for i, d in enumerate(dets):
> 
> x1 = d.top() if d.top() > 0 else 0
> 
> y1 = d.bottom() if d.bottom() > 0 else 0
> 
> x2 = d.left() if d.left() > 0 else 0
> 
> y2 = d.right() if d.right() > 0 else 0
> 
> \# img\[y:y+h,x:x+w\]
> 
> face = img\[x1:y1,x2:y2\]
> 
> \# 调整图片的尺寸
> 
> face = cv2.resize(face, (size,size))
> 
> cv2.imshow('image',face)
> 
> \# 保存图片
> 
> cv2.imwrite(output\_dir+'/'+str(index)+'.jpg', face)
> 
> index += 1
> 
> key = cv2.waitKey(30) & 0xff
> 
> if key == 27:
> 
> sys.exit(0)

这个项目用到的图片数是10000张左右，如果是自己下载的图片集，控制一下图片的数量避免数量不足，或图片过多带来的内存不够与运行缓慢。

4、训练模型

有了训练数据之后，通过cnn来训练数据，就可以让她记住我的人脸特征，学习怎么认识我了。

> train\_faces.py
> 
> import tensorflow as tf
> 
> import cv2
> 
> import numpy as np
> 
> import os
> 
> import random
> 
> import sys
> 
> from sklearn.model\_selection import train\_test\_split
> 
> my\_faces\_path = './my\_faces'
> 
> other\_faces\_path = './other\_faces'
> 
> size = 64
> 
> imgs = \[\]
> 
> labs = \[\]
> 
> def getPaddingSize(img):
> 
> h, w, \_ = img.shape
> 
> top, bottom, left, right = (0,0,0,0)
> 
> longest = max(h, w)
> 
> if w < longest:
> 
> tmp = longest - w
> 
> \# //表示整除符号
> 
> left = tmp // 2
> 
> right = tmp - left
> 
> elif h < longest:
> 
> tmp = longest - h
> 
> top = tmp // 2
> 
> bottom = tmp - top
> 
> else:
> 
> pass
> 
> return top, bottom, left, right
> 
> def readData(path , h=size, w=size):
> 
> for filename in os.listdir(path):
> 
> if filename.endswith('.jpg'):
> 
> filename = path + '/' + filename
> 
> img = cv2.imread(filename)
> 
> top,bottom,left,right = getPaddingSize(img)
> 
> \# 将图片放大， 扩充图片边缘部分
> 
> img = cv2.copyMakeBorder(img, top, bottom, left, right, cv2.BORDER\_CONSTANT, value=\[0,0,0\])
> 
> img = cv2.resize(img, (h, w))
> 
> imgs.append(img)
> 
> labs.append(path)
> 
> readData(my\_faces\_path)
> 
> readData(other\_faces\_path)
> 
> \# 将图片数据与标签转换成数组
> 
> imgs = np.array(imgs)
> 
> labs = np.array(\[\[0,1\] if lab == my\_faces\_path else \[1,0\] for lab in labs\])
> 
> \# 随机划分测试集与训练集
> 
> train\_x,test\_x,train\_y,test\_y = train\_test\_split(imgs, labs, test\_size=0.05, random\_state=random.randint(0,100))
> 
> \# 参数：图片数据的总数，图片的高、宽、通道
> 
> train\_x = train\_x.reshape(train\_x.shape\[0\], size, size, 3)
> 
> test\_x = test\_x.reshape(test\_x.shape\[0\], size, size, 3)
> 
> \# 将数据转换成小于1的数
> 
> train\_x = train\_x.astype('float32')/255.0
> 
> test\_x = test\_x.astype('float32')/255.0
> 
> print('train size:%s, test size:%s' % (len(train\_x), len(test\_x)))
> 
> \# 图片块，每次取100张图片
> 
> batch\_size = 100
> 
> num\_batch = len(train\_x) // batch\_size
> 
> x = tf.placeholder(tf.float32, \[None, size, size, 3\])
> 
> y\_ = tf.placeholder(tf.float32, \[None, 2\])
> 
> keep\_prob\_5 = tf.placeholder(tf.float32)
> 
> keep\_prob\_75 = tf.placeholder(tf.float32)
> 
> def weightVariable(shape):
> 
> init = tf.random\_normal(shape, stddev=0.01)
> 
> return tf.Variable(init)
> 
> def biasVariable(shape):
> 
> init = tf.random\_normal(shape)
> 
> return tf.Variable(init)
> 
> def conv2d(x, W):
> 
> return tf.nn.conv2d(x, W, strides=\[1,1,1,1\], padding='SAME')
> 
> def maxPool(x):
> 
> return tf.nn.max\_pool(x, ksize=\[1,2,2,1\], strides=\[1,2,2,1\], padding='SAME')
> 
> def dropout(x, keep):
> 
> return tf.nn.dropout(x, keep)
> 
> def cnnLayer():
> 
> \# 第一层
> 
> W1 = weightVariable(\[3,3,3,32\]) \# 卷积核大小(3,3)， 输入通道(3)， 输出通道(32)
> 
> b1 = biasVariable(\[32\])
> 
> \# 卷积
> 
> conv1 = tf.nn.relu(conv2d(x, W1) + b1)
> 
> \# 池化
> 
> pool1 = maxPool(conv1)
> 
> \# 减少过拟合，随机让某些权重不更新
> 
> drop1 = dropout(pool1, keep\_prob\_5)
> 
> \# 第二层
> 
> W2 = weightVariable(\[3,3,32,64\])
> 
> b2 = biasVariable(\[64\])
> 
> conv2 = tf.nn.relu(conv2d(drop1, W2) + b2)
> 
> pool2 = maxPool(conv2)
> 
> drop2 = dropout(pool2, keep\_prob\_5)
> 
> \# 第三层
> 
> W3 = weightVariable(\[3,3,64,64\])
> 
> b3 = biasVariable(\[64\])
> 
> conv3 = tf.nn.relu(conv2d(drop2, W3) + b3)
> 
> pool3 = maxPool(conv3)
> 
> drop3 = dropout(pool3, keep\_prob\_5)
> 
> \# 全连接层
> 
> Wf = weightVariable(\[8\*16\*32, 512\])
> 
> bf = biasVariable(\[512\])
> 
> drop3\_flat = tf.reshape(drop3, \[-1, 8\*16\*32\])
> 
> dense = tf.nn.relu(tf.matmul(drop3\_flat, Wf) + bf)
> 
> dropf = dropout(dense, keep\_prob\_75)
> 
> \# 输出层
> 
> Wout = weightVariable(\[512,2\])
> 
> bout = weightVariable(\[2\])
> 
> \#out = tf.matmul(dropf, Wout) + bout
> 
> out = tf.add(tf.matmul(dropf, Wout), bout)
> 
> return out
> 
> def cnnTrain():
> 
> out = cnnLayer()
> 
> cross\_entropy = tf.reduce\_mean(tf.nn.softmax\_cross\_entropy\_with\_logits(logits=out, labels=y\_))
> 
> train\_step = tf.train.AdamOptimizer(0.01).minimize(cross\_entropy)
> 
> \# 比较标签是否相等，再求的所有数的平均值，tf.cast(强制转换类型)
> 
> accuracy = tf.reduce\_mean(tf.cast(tf.equal(tf.argmax(out, 1), tf.argmax(y\_, 1)), tf.float32))
> 
> \# 将loss与accuracy保存以供tensorboard使用
> 
> tf.summary.scalar('loss', cross\_entropy)
> 
> tf.summary.scalar('accuracy', accuracy)
> 
> merged\_summary\_op = tf.summary.merge\_all()
> 
> \# 数据保存器的初始化
> 
> saver = tf.train.Saver()
> 
> with tf.Session() as sess:
> 
> sess.run(tf.global\_variables\_initializer())
> 
> summary\_writer = tf.summary.FileWriter('./tmp', graph=tf.get\_default\_graph())
> 
> for n in range(10):
> 
> \# 每次取128(batch\_size)张图片
> 
> for i in range(num\_batch):
> 
> batch\_x = train\_x\[i\*batch\_size : (i+1)\*batch\_size\]
> 
> batch\_y = train\_y\[i\*batch\_size : (i+1)\*batch\_size\]
> 
> \# 开始训练数据，同时训练三个变量，返回三个数据
> 
> \_,loss,summary = sess.run(\[train\_step, cross\_entropy, merged\_summary\_op\],
> 
> feed\_dict=\{x:batch\_x,y\_:batch\_y, keep\_prob\_5:0.5,keep\_prob\_75:0.75\})
> 
> summary\_writer.add\_summary(summary, n\*num\_batch+i)
> 
> \# 打印损失
> 
> print(n\*num\_batch+i, loss)
> 
> if (n\*num\_batch+i) % 100 == 0:
> 
> \# 获取测试数据的准确率
> 
> acc = accuracy.eval(\{x:test\_x, y\_:test\_y, keep\_prob\_5:1.0, keep\_prob\_75:1.0\})
> 
> print(n\*num\_batch+i, acc)
> 
> \# 准确率大于0.98时保存并退出
> 
> if acc > 0.98 and n > 2:
> 
> saver.save(sess, './train\_faces.model', global\_step=n\*num\_batch+i)
> 
> sys.exit(0)
> 
> print('accuracy less 0.98, exited!')
> 
> cnnTrain()
> 
> 训练之后的数据会保存在当前目录下。
> 
> 5、使用模型进行识别
> 
> 最后就是让她认识我了，很简单，只要运行程序，让摄像头拍到我的脸，她就可以轻松地识别出是不是我了。
> 
> is\_my\_face.py
> 
> output = cnnLayer()
> 
> predict = tf.argmax(output, 1)
> 
> saver = tf.train.Saver()
> 
> sess = tf.Session()
> 
> saver.restore(sess, tf.train.latest\_checkpoint('.'))
> 
> def is\_my\_face(image):
> 
> res = sess.run(predict, feed\_dict=\{x: \[image/255.0\], keep\_prob\_5:1.0, keep\_prob\_75: 1.0\})
> 
> if res\[0\] == 1:
> 
> return True
> 
> else:
> 
> return False
> 
> \#使用dlib自带的frontal\_face\_detector作为我们的特征提取器
> 
> detector = dlib.get\_frontal\_face\_detector()
> 
> cam = cv2.VideoCapture(0)
> 
> while True:
> 
> \_, img = cam.read()
> 
> gray\_image = cv2.cvtColor(img, cv2.COLOR\_BGR2GRAY)
> 
> dets = detector(gray\_image, 1)
> 
> if not len(dets):
> 
> \#print('Can\`t get face.')
> 
> cv2.imshow('img', img)
> 
> key = cv2.waitKey(30) & 0xff
> 
> if key == 27:
> 
> sys.exit(0)
> 
> for i, d in enumerate(dets):
> 
> x1 = d.top() if d.top() > 0 else 0
> 
> y1 = d.bottom() if d.bottom() > 0 else 0
> 
> x2 = d.left() if d.left() > 0 else 0
> 
> y2 = d.right() if d.right() > 0 else 0
> 
> face = img\[x1:y1,x2:y2\]
> 
> \# 调整图片的尺寸
> 
> face = cv2.resize(face, (size,size))
> 
> print('Is this my face? %s' % is\_my\_face(face))
> 
> cv2.rectangle(img, (x2,x1),(y2,y1), (255,0,0),3)
> 
> cv2.imshow('image',img)
> 
> key = cv2.waitKey(30) & 0xff
> 
> if key == 27:
> 
> sys.exit(0)
> 
> sess.close()

写在最后作者的话，这段时间正在学习Tensorflow的卷积神经网络部分，为了对卷积神经网络能够有一个更深的了解，自己动手实现一个例程是比较好的方式，所以就选了一个这样比较有点意思的项目。

关注基数智能公众号jishu2017ai，一起加入AI技术大本营。


[Tensorflow]: /pro/os/crawler/NUBV-FYBV-UMRN.jpg
 *  **原文作者：** 基数智能
 *  **原文链接：** https://www.toutiao.com/item/6483702092354552334/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。