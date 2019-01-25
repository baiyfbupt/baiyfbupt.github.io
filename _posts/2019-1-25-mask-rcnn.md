---
layout:     post
title:      "2stage目标检测（四）：Mask R-CNN"
subtitle:   "论文阅读笔记"
date:       2019-1-25 16:30:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers






---

# Mask R-CNN

## 写在前面

Mask R-CNN是ICCV-2017的Best Paper，该算法在单GPU上达到了5fps的速度，并且在COCO数据集的三个挑战赛：instance segmentation、bounding-box object detecton、person keypoint detection中的效果都要优于现有的单模型算法

文章的创新点主要有三个：

1. 在Faster RCNN的基础上进行了扩展，新引入了一个并行FCN分支用于实例分割
2. 引入了ROIAlign，消除了原来两次量化产生的误差
3. Loss function

## 主要思想

1. FCN分支

   ![mask-rcnn](/img/post/mask-rcnn.jpg)

   Mask-RCNN新引入了FCN的Mask分支

2. ROIAlign

   引入了双线性插值，避免了量化带来的误差问题

3. Loss function

   网络中的loss L为分类，bbox和mask loss之和

   $$L=L_{cls}+L_{box}+L_{mask}$$

   每个 ROIAlign 对应 K * m^2 维度的输出。K 对应类别个数，即输出 K 个mask，m对应 池化分辨率（7*7）。Loss 函数定义：

   $$L_{mask}(cls_k) = Sigmoid (cls_k)$$

   平均二值交叉熵 （average binary cross-entropy）Loss，通过逐像素的 Sigmoid 计算得到。
   为什么用K个mask？通过对每个 Class 对应一个 Mask 可以有效避免类间竞争（其他 Class 不贡献 Loss ）。

## 开源代码

作者在[Detectron](https://github.com/ facebookresearch/Detectron)开源了论文使用的代码

## 参考文献

1. [Mask R-CNN](https://arxiv.org/pdf/1703.06870.pdf)
2. [Mask RCNN笔记](https://blog.csdn.net/xiamentingtao/article/details/78598511)

