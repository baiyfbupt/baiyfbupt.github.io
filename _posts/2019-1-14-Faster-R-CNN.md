---
layout:     post
title:      "2stage目标检测（三）：Faster R-CNN"
subtitle:   "论文阅读笔记"
date:       2019-1-14 15:30:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers





---

# Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks

## 写在前面

Faster R-CNN发表在ECCV-2016上，是RCNN系列的有一部加强版

本文推出了RPN候选区生成网络，代替了传统的Selective search，该网络与Fast R-CNN网络共享一部分参数，可以完成端到端的训练

## 主要思想

作者开篇提到本文的动机，在Fast R-CNN已经改进了预测部分的速度以后，生成候选区域成了整个目标检测系统的性能瓶颈，所以推出了一种类似于全卷积网络的RPN来生成候选区，将原来串行运行在CPU上的部分可以在GPU并行加速

至此2stage目标检测网络已经由原来的三部分合成了现在的一部分，如下图所示

![RCNN-develop](/img/post/RCNN-develop.jpg)

使用Faster R-CNN进行目标检测可以分为以下几步：

![Faster-detail](/img/post/Faster-detail.png)

1. 首先对原始图片进行裁剪操作，将裁剪后的图片送进CNN中获取该图像对应的特征图
2. 对特征图上的每一个锚点上选取9个候选的ROI（3个不同尺度，3个不同长宽比），并根据相应比例映射到原始图象中
3. 随后将这些ROI送进RPN网络中，RPN网络对这些ROI进行二分类，同时进行回归（包括$ \Delta x, \Delta y, \Delta w, \Delta h$）,然后对ROI做NMS，选取其中最好的K个ROI
4. 然后对不同大小的ROI进行ROI Pooling，输出固定大小的feature map
5. 最后将其输入简单的检测网络中，最后完成BB的分类与回归，输出检测结果BB

## 一些概念

- 锚点

  即特征图的最小单位点，比如原始图像大小是256\*256，CNN包含4个Pool层，最终得到的特征图大小是16\*16，最小单位即是锚点，该特征图包含256个锚点

- 候选ROI

  针对每一个锚点，都根据不同尺度(128,256,512像素)和不同长宽比(1:1,0.5:1,1:0.5)产生9个BB，所以该特征图共包含9\*16\*16个候选ROI

- RPN网络

  RPN网络用来对特征图上的ROI进行二分类和定位，推选出最终的ROI，此时的ROI是相对于原始图像的

- ROI Pooling

  ROI Pooling使用最大值池化将上一步得到的ROI固定到特定大小的特征图Feature map上,以便进行最后的分类和回归操作

  **ROI Pooling的一个不足**，由于预选ROI的位置通常是有模型回归得到的，一般来说是浮点数，而池化后的特征图要求尺度固定，因此ROI Pooling这个操作存在两次数据量化的过程。1）将候选框边界量化为整数点坐标值；2）将量化后的边界区域平均分割成kxk个单元，对每个单元的边界进行量化。事实上，经过上面的两次量化操作，此时的ROI已经和最开始的ROI之间存在一定的偏差，这个偏差会影响检测的精确度。

- 正负样本的划分

  1. 对每个标定的GT box，与其重叠比例最大的anchor记为 正样本 (保证每个ground true 至少对应一个正样本anchor)；
  2. 对1)剩余的anchor，如果其与某个标定区域重叠比例大于0.7，记为正样本（每个ground true box可能会对应多个正样本anchor。但每个正样本anchor 只可能对应一个grand true box）；如果其与任意一个标定的重叠比例都小于0.3，记为负样本。
  3. 对上面两步剩余的anchor，弃去不用；跨越图像边界的anchor弃之不用

- 训练策略

  Faster R-CNN的训练可以分为四步：

  1. 使用ImageNet预训练，训练一个RPN网络
  2. 用RPN生成的ROI训练一个Fast R-CNN网络
  3. 使用第2步得到的网络重新训练一个RPN网络，但是将共享的CNN部分学习率设置为0，仅训练RPN独有的部分
  4. 使用新的RPN生成的ROI继续fine-tune检测网络中特有的网络层

## 开源代码

[caffe](https://github.com/rbgirshick/py-faster-rcnn)

## 参考文献

1. [Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks](https://arxiv.org/pdf/1506.01497.pdf)
2. [Faster-rcnn详解](https://blog.csdn.net/wzz18191171661/article/details/79439212)