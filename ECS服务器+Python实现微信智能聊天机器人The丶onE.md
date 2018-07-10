---
title: ECS服务器+Python实现微信智能聊天机器人
date: 2018-06-06 21:21:55
categories: "开发"
tags:
	- 文本编辑器
	- 移动互联网
	- 微信
	- 编程语言
	- Python

---

# **前言** #

这周末“闲来无事”（其实我下周要考试的…），想做一个微信自动回复聊天机器人（免得天天有人说我不及时回消息…）。要实现这个功能有两种方案：一是微信自动回复设置好的语句，我称为简单自动回复，就像大家熟知的QQ自动回复一样；二是微信根据消息内容自动回复，我称为智能自动回复，这就是所谓的智能聊天机器人，就像QQ小冰一样。要实现第二种方案，也有两种选择，一是自己用深度学习训练一个机器人，二是直接调用其他已经训练好的机器人。今天，我们来做一个微信智能聊天机器人，由于自己训练一个机器人难度较大，我们以后再讨论，今天先讨论调用其他机器人来实现微信智能聊天机器人。

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python]

# **工具** #

``````````
系统：Ubuntu 16.04编程语言：python 2.7机器人API：图灵机器人
``````````

因为我想要脚本24小时运行，所以我用的是服务器。大家实验可以不用服务器，系统也可以不用Ubuntu，也可以不用Python 2.7，机器人也可以不用图灵机器人…总之大家开心就好，本文只是通过一个例子来讨论思路。

# **获取机器人APIkey** #

首先我们需要去图灵机器人注册一个账号，过程不赘述，然后如下图创建机器人和获取APIkey。

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 1]

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 2]

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 3]

# **编写代码** #

首先我们需要登录我们的服务器，在终端输入以下代码：

新建一个pyrobot文件夹：mkdir pyrobot

进入 pyrobot 文件夹：cd pyrobot

在 pyrobot 文件夹中新建一个robot.py:vi robot.py

这时vim会在pyrobot中创建一个robot.py文件，按i进入vim的编辑模式,然后输入以下代码，输入完成之后，按esc，再按:wq保存文件。

``````````
注意：记得把下面代码中第49行的”你的APIkey”字段替换为刚才获取到的APIkey#coding=utf8import itchatfrom itchat.content import TEXTfrom itchat.content import *import sysimport timeimport reimport requests, jsonimport aimlimport os# When recieve the following msg types, trigger the auto replying.@itchat.msg_register([TEXT, PICTURE, FRIENDS, CARD, MAP, SHARING, RECORDING, ATTACHMENT, VIDEO],isFriendChat=True, isMpChat=True)def text_reply(msg): global auto_reply, robort_reply, peer_list # The command signal of "[自动回复]"  if msg['FromUserName'] == myUserName and msg['Content'] == u"开启自动回复": auto_reply = True itchat.send_msg(u"[自动回复]已经打开。
", msg['FromUserName']) elif msg['FromUserName'] == myUserName and msg['Content'] == u"关闭自动回复": auto_reply = False itchat.send_msg(u"[自动回复]已经关闭。
", msg['FromUserName']) # elif not msg['FromUserName'] == myUserName:  else:  if auto_reply == True: itchat.send_msg(u"[自动回复]您好，我是少林寺驻武当山办事处大神父王喇嘛，我伟大的主人现在不在，我会将你的话转告给他哟。
", msg['FromUserName']) else:  ''' For none-filehelper message, if recieve '= =', start robort replying. if recieve 'x x', stop robort replying. ''' if msg['Content'] == u"= =": robort_reply = True peer_list.append(msg['ToUserName']) return elif msg['Content'] == u"x x": robort_reply = False peer_list.remove(msg['ToUserName']) return  # Let Turing reply the msg. if robort_reply == True and msg['FromUserName'] in peer_list: # Sleep 1 second is not necessary. Just cheat human.  time.sleep(1)  cont = requests.get('http://www.tuling123.com/openapi/api?key=你的APIkey&info=%s' % msg['Content']).content m = json.loads(cont) itchat.send(m['text'], msg['FromUserName']) if m['code'] == 200000: itchat.send(m['url'], msg['FromUserName']) if m['code'] == 302000: itchat.send(m['list'], msg['FromUserName']) if m['code'] == 308000: itchat.send(m['list'], msg['FromUserName']) return# Mainif __name__ == '__main__': # Set the hot login itchat.auto_login(enableCmdQR=2, hotReload=True) # Get your own UserName myUserName = itchat.get_friends(update=True)[0]["UserName"] print myUserName auto_reply = False robort_reply = False peer_list = [] itchat.run()
``````````

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 4]

下面简单谈下上面代码实现的效果:

首先是我们通过微信给他人发送“开启自动回复”或者“关闭自动回复”，会开启或者关闭简单自动回复，这个简单回复是针对除了微信群外的所有回复的；

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 5]

然后我们通过微信给他人发送“= =”或“x x”开启或者关闭智能自动回复，这个智能回复仅针对你发送了指令的微信用户，简单说，智能自动回复可以对特定用户开启或者关闭。

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 6]

如果不喜欢上面设置的开启或者关闭自动回复指令，可以修改第35行和第39行代码为自己喜欢的指令。

# **启动脚本** #

现在我们已经写好了代码，我们需要启动脚本。观察上面的代码，我们发现使用了itchat和 aiml库，所以我们首先需要安装这两个库。

首先在终端输入：cd -返回root目录

依次输入下面的代码：

安装itchat库：pip install itchat

安装aiml库：pip install aiml

如果无法用此方式安装库，可能是没装pip，输入sudo apt-get install python-pip安装即可，升级pip用pip install -U pip。

安装好了库，现在我们就可以来启动脚本了！

输入：cd pyrobot进入pyrobot文件夹，然后再输入：python robot.py启动脚本。

启动脚本后，会生成一个二维码，我们需要用微信扫描这个二维码进行登录，登录成功后，脚本就开始运行了！

![ECS服务器+Python实现微信智能聊天机器人][ECS_Python 7]

然后我们就可以开始找人实验，观看效果了…

**温馨提示：千万不要拿亲人做实验，后果很严重！！！**

# **参考资料：** #

【微信】设置自动回复消息和智能聊天

Python & aiml 搭建聊天机器人

python入门第一课，最简单的python编写

A complete and graceful API for Wechat. 微信个人号接口、微信机器人及命令行微信，三十行即可自定义个人号机器人。

Python的包管理工具pip的安装与使用


[ECS_Python]: /pro/os/crawler/6VU6-VRZA-3ARU.jpg
[ECS_Python 1]: /pro/os/crawler/M7FF-JUMA-JMQ2.jpg
[ECS_Python 2]: /pro/os/crawler/ZEYM-YBY2-2QQI.jpg
[ECS_Python 3]: /pro/os/crawler/ZUF7-JNAA-QJNJ.jpg
[ECS_Python 4]: /pro/os/crawler/ZVZE-MZ3E-6NYA.jpg
[ECS_Python 5]: /pro/os/crawler/NYU2-QZMQ-AAZZ.jpg
[ECS_Python 6]: /pro/os/crawler/RIYZ-63EJ-7JE3.jpg
[ECS_Python 7]: /pro/os/crawler/F6BJ-BZQN-BJYF.jpg
 *  **原文作者：** The丶onE
 *  **原文链接：** https://www.toutiao.com/item/6563961218569077262/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。