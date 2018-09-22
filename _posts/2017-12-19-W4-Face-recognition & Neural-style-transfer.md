---
layout:     post
title:      "Convolutional Neural Networks 第四周笔记"
subtitle:   "Face recognition&Neural style transfer"
date:       2017-12-19 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Deep Learning
    - Course
---

# Face recognition&Neural style transfer

## Face recognition

### Verification&Recognition

* Verification：验证1对1的关系是否正确
* Recognition：从数据库中识别出一个目标，是1对K的关系

### One-shot学习

> Learn from one example to recognize the person again

**similarity**函数

输入两张图片img1,img2，输出两张图片差异的程度，若差异足够小$$d(img1,img2)<t$$，则认为两张图片相同

**Siamese network**

![Siamese network](/img/post/siamese_network.jpg)

训练网络使同一个人的embeddings:$$f(x^{(i)})，f(x^{(j)})$$相差尽量小

### Triplet Loss

三元损失函数基于A,P,N三张图片，A测试图片，P正确图片，N错误图片

$$d(A,P) - d(A,N) + \alpha \leq0$$

$$L(A,P,N) = max(||f(A) - f(P)||^2 - ||f(A) - f(N)||^2 +\alpha , 0)$$

$$J = \sum_{i=1}^mL(A^{(i)},P^{(i)},N^{(i)})$$

## Neural style transfer

### cost function

对于图片C要转换到风格S,生成图片G,定义成本函数为

$$J(G) = \alpha J_{Content}(C,G) + \beta J_{Style}(S,G)$$

### 产生图片G的步骤

1. 随机初始化G:100 x 100 x3
2. 用梯度下降最小化J(G)：$$G := G - \frac \alpha{\alpha G}J(G)$$

### Content cost Function

取神经网络中间的隐藏层l计算内容损失Content cost

$$J_{Contnent}(C,G) = \frac1n||a^{[l](C)} - a^{[l](G)}||^2$$

### Style cost Function

取神经网络隐藏层l,定义style为不同通道之间的相关性

GRAM matrix:

$$G^{[l](S)}_{kk'} = \sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{i,j,k}^{[l](S)}a_{i,j,k'}^{[l](S)}$$

$$G^{[l](G)}_{kk'} = \sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{i,j,k}^{[l](G)}a_{i,j,k'}^{[l](G)}$$

$$J_{style}^{[l]}(S,G) = \frac1{(2*n_H^{[l]}*n_W^{[l]}*n_C^{[l]})^2}\sum_k\sum_{k'}(G^{[l](S)}_{kk'} - G^{[l](G)}_{kk'} )^2$$

$$J_{style}(S,G) = \sum_l\lambda^{[l]}J^{[l]}_{style}(S,G)$$

### 1D&3D Convolutions

1 x 14 conv 1 x 5 ----> 1 x 10 

14 x 14 x14 x1 conv 5 x 5 x 5 x1 ---->10 x 10 x 10 x 1