---
layout:     post
title:      "两阶段目标检测（二）：Fast R-CNN"
subtitle:   "论文阅读笔记"
date:       2019-1-10 15:15:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers




---

# Fast R-CNN

## 写在前面

这篇论文出自RGB，发表在ICCV-2015上，是R-CNN与SPPnet的改进版。相比于RCNN，FRCN在训练阶段快9倍，测试阶段快213倍。相比于SPPnet，FRCN在训练阶段快3倍，测试阶段快10倍，并且准确率也有了一定的提高。

## 主要思想

文章开篇就指出了之前方法的几个不足：

1. 训练分了多个阶段组成，因为网络中既有ConvNets也有SVM，非常麻烦
2. 训练非常的慢，而且会额外占用硬盘空间
3. 检测速度很慢

本文提出的Fast R-CNN计算过程如下图所示

![Fast-RCNN](/img/post/Fast-RCNN.jpg)

Fast R-CNN的贡献点可以归结为两点：

1. 实现了大部分的端到端训练，用Softmax代替了SVM

2. 提出了ROI pooling层，可以不用每个proposal都做一次前向计算了

部分细节：

- 论文使用了smooth l1作为回归loss，避免了l2容易梯度爆炸的问题
- 论文将比较大的全链接层用SVD分解了一下使得检测的时候更加迅速。

## 开源代码

作者开源了他的代码：[Caffe](https://github.com/rbgirshick/fast-rcnn)

## 参考文献

1. [Fast R-CNN](https://arxiv.org/pdf/1504.08083.pdf)

2. [Fast R-CNN详解](https://zhuanlan.zhihu.com/p/24780395)