---
layout:     post
title:      "Convolutional Neural Networks 第一周笔记"
subtitle:   "Convolutional Neural Networks"
date:       2017-11-22 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Course
---

# Convolutional Neural Networks

> Computer Vision Problems:Image Classification;Object detection;Neural Style Transfer

## Edge Detection

Vertical edge detection

垂直边缘filter
$$
\left[
\begin{matrix}
   1 & 0 & -1 \\
   1 & 0 & -1 \\
   1 & 0 & -1
  \end{matrix} 
  \right] \tag{1}
$$
Horizontal edge detection

水平边缘filter
$$
\left[
\begin{matrix}
    1 &  1 &  1 \\
    0 &  0 &  0 \\
   -1 & -1 & -1
  \end{matrix} 
  \right] \tag{2}
$$

## Padding

* **Valid Conv**:对一幅$$(n,n)$$的图像用$$(f,f)$$的滤波器卷积，得到的结果图像尺寸是$$(n-f+1,n-f+1)$$
* **Same Conv(pad)**:对一幅$$(n,n)$$的图像先pad到$$(n+2,n+2)$$,p=2，再与$$(f,f)$$的滤波器卷积，得到的结果仍然是$$(n,n)$$

$$n+2p-f+1=n, p=(f-1)/2$$

## Stride Convolutions

对一幅$$(n*n)$$的图像，用$$(f*f)$$的滤波器做padding = p,stride=s的卷积，得到的结果是
$$(\frac{n+2p-f}{s}+1,\frac{n+2p-f}{s}+1)$$

卷积？互相关？

## 高维卷积

例如3维图像与3维滤波器卷积$$(6,6,3)*(3,3,3) = (4,4)$$

$$(n,n,n_c) * (f,f,n_c) = (n-f+1,n-f+1)...n_c​$$应保持一致

### convolutional network

* Convolution
* Pooling
* Fully connected

## Pooling

* Max pooling:取区域内最大值
* Avg pooling:取区域内平均值

### Hyperparameters:

* f:filter size
* s:stride
* Max or average pooling