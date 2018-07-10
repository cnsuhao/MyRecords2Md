---
title: 利用python简单绘图——自动绘制树林
date: 2018-01-28 20:36:04
categories: "开发"
tags:
	- 镜音双子
	- 技术
	- Branch
	- 编程语言
	- Python

---

![利用python简单绘图——自动绘制树林][python]

运行示例

**1、使用版本：python3.6**

**2、适合python初学者，感兴趣的可以自己动手敲一遍代码哦。**

**3、 绘制树林**

**实现方法**：

将每棵树的绘制以maketree函数封装，参数x,y为 画树的起点位置即树根位置。在main函数中只要以 不同的参数设置来调用maketree函数就可以完成多 棵树的绘制了

# 代码示例： #

\# drawtree.py

from turtle import Turtle, mainloop

def tree(plist, l, a, f):

""" plist is list of pens

l is length of branch

a is half of the angle between 2 branches

f is factor by which branch is shortened

from level to level."""

if l > 5:

lst = \[\]

for p in plist:

p.forward(l)

q = p.clone()

p.left(a)

q.right(a)

lst.append(p)

lst.append(q)

tree(lst, l\*f, a, f)

\#森林的绘制

def maketree(x,y):

p = Turtle()

p.color("green")

p.pensize(5)

p.hideturtle()

p.speed(10)

p.left(90)

p.penup()

p.goto(x,y)

p.pendown()

t = tree(\[p\],200,65,0.6375)

print(len(p.getscreen().turtles()))

def main():

maketree(-200,-200)

maketree(0,0)

maketree(200,-200)

main()

# 运行效果： #

![利用python简单绘图——自动绘制树林][python 1]

运行示例图


[python]: /pro/os/crawler/FIBZ-RYVF-AFQR.gif
[python 1]: /pro/os/crawler/U7VU-UBBU-QUIE.jpg
 *  **原文作者：** Python程序员
 *  **原文链接：** https://www.toutiao.com/item/6516079413363212803/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。