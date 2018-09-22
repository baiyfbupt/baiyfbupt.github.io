---
layout:     post
title:      "Convolutional Neural Networks 第二周笔记"
subtitle:   "Case studies"
date:       2017-11-24 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Deep Learning
    - Course
---

# Case studies

## Classical Networks

### LeNet - 5

**input(32,32,1)**----*6filters(5x5,s=1)*---->**conv1(28,28,6)**----*avg pool(f=2,s=2)*---->**pool1(14,14,6)**----*16filters(5x5,s=1)*---->**conv2(10,10,16)**----avg pool(f=2,s=2)---->**pool2(5,5,16)**---->**fully connect(120 neurons)**---->**fully connect(84 neurons)**

### AlexNet

### VGG - 16

### Residual Networks（残差网络）

short cut 穿过Residual Network，有助于**解决深度神经网络的梯度消失和梯度爆炸**的问题

### Network in Network(1x1 convolution)

shrink the channels or increase it or keep it

压缩网络的通道数形成整个网络的瓶颈层，对缩减所需要的计算量有很大帮助

### Inception Network

将多种滤波器或者池化（1x1,3x3,5x5,MAX-POOL）的结果堆砌起来

![Inception](/img/post/Inception.png)

## Transfer Learning

下载开源神经网络和预先训练好的权重，只用自己的数据集重新训练最后几层（自己的数据集越多就适合训练越多的神经层）

## Data augmentation

* Mirroring
* Random Cropping:随机裁剪
* *Rotation*
* *Shearing*
* *Local wraping*
* Color shifting