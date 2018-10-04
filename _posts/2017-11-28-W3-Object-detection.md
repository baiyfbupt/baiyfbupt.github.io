---
layout:     post
title:      "Convolutional Neural Networks 第三周笔记"
subtitle:   "Object detection"
date:       2017-11-28 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Course
---

# Object detection

## Object Localization

对于一个识别类别的神经网络

1. pedestrian
2. car
3. motorcycle
4. background

同时输出￥$b_x,b_y,b_h,b_w$￥和类别标签(1-4)

定义标签$$y = [P_c,b_x,b_y,b_h,b_w,C_1,C_2,C_3]$$

**$$b_x,b_y$$代表边界框的中心点，$$b_h,b_w$$代表边界框的高度和宽度**

$$P_c$$表示图片中是否有任意目标（1-4）

$$C_1,C_2,C_3$$分别表示是否有pedestrian,car,motorcycle

**损失函数**(以平方差举例)

$$L(\hat y,y) = (\hat y_1 - y_1)^2 + (\hat y_2 - y_2)^2 + ... + (\hat y_8 - y_8)^2 \  \ \ \    if\  y_1 = 1$$

$$L(\hat y,y) = (\hat y_1 - y_1)^2 \ \ \ if\  y_1 = 1$$

## Landmark Detection

为识别特定的图像可以在训练集图像上标记需要识别图像的landmark特征点，需要注意的是同一个landmark m需要在所有训练图片上保持一致

## Objection Detection

### Sliding windows detection

1. 定义一个滑动窗口，以固定的步长对一张图片进行遍历，分别经过卷积神经网络后判断是否存在特定的物体以0,1分类
2. 重复上述操作，但是重新定义一个较大的窗口
3. 重复上述操作，但是重新定义一个较大的窗口

滑动窗口检测对计算力的要求非常高

新的算法将全连接层FC用卷积实现，可以较少重复计算

![Screenshot from 2017-11-24 17-18-56](/img/post/sliding_window.jpg)

### Bounding Box Predictions

**YOLO**

### 交并比函数(IoU)

IoU函数可以用来评价检测算法的性能表现，基本思想是计算预测区域与标签区域的交集A与并集B的面积之比

$$IoU = \frac{S_A}{S_B}$$

IoU>0.5 则认为识别正确

### Non-max suppression alogorithm(NMS)

### Anchor Boxes

为了在同一个格子中能够检出多个重叠的物体，有了Anchor Boxes方法

Anchor Boxes基本思想是定义两个Anchor box 在标签y中把两个anchor box联合起来

### Region proposal:R-CNN

R-CNN是先经过segmental algorithm，照片分类为很多子目标再把子目标拿去卷积，用的是传统的滑动窗口算法，即每一个窗口分别进行卷积

![Screenshot from 2017-12-19 11-29-37](/img/post/rcnn.jpg)





