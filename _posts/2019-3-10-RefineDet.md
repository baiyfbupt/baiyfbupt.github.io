---
layout:     post
title:      "RefineDet-结合了单阶段和双阶段优点的目标检测器"
subtitle:   "论文阅读笔记"
date:       2019-3-10 12:00:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers


---

# Single-Shot Reﬁnement Neural Network for Object Detection

## 写在前面

本文发表在CVPR-2018，是中科院的一篇关于目标检测的论文

现阶段目标检测分为，单阶段和双阶段两种方式，双阶段精度高但速度慢，单精度速度快但精度稍逊。本文作者就想提出一种新的融合的方法，将两种目标检测方法的优点结合起来

本文模型在COCO测试集上取得了`AP41.8`的性能表现，`320x320`速度为`40.2FPS`，`512x512`速度为`24.1FPS`

## 主要思想

作者认为，双阶段目标检测方法相比单阶段目标检测方法有以下三个优势：

1. 双阶段目标检测器采用了两段结构`采样`来处理类别不均衡的问题
2. 使用了两阶段`级联`的方式来拟合bbox
3. 采用了`两阶段的特征图`来描述待检目标

作者推出的RefineDet整体结构如下图所示：

![RefineDet](/img/post/RefineDet.jpg)

模型主要由两个部分组成，`anchor reﬁnement module(ARM)`和`object detection module(ODM)`,分别有各自的损失函数，两部分通过级联的方式依次处理目标框，作者以这种端到端的多任务学习方法代替R-CNN系列目标检测的两阶段方法。

## 关键点

1. ARM

   ARM在模型中的作用主要有以下两个：

   - 粗筛`anchor`，剔除掉过于容易的负样本，降低后续的计算复杂度

   - 粗调`anchor`的边界，为`ODM`提供更好的初始值

2. `transfer connection block (TCB)`

   TCB是为了融合ARM与ODM之间各个层级的特征，不仅可以传递`anchor`的信息，也是一种做特征金字塔的方式

   本文作者使用了反卷积和按位加法来完成了TCB的运算，计算过程如下图所示

   ![TCB](/img/post/TCB.jpg)

3. `two-step cascaded regression`

   作者认为目前的单阶段目标检测器只进行了一次的目标框回归，这可能是导致在一些困难任务上表现不佳的原因

   所以，不同于SSD，`RefineDet`采用了两步的回归策略，首先由`ARM`生成大致的目标框，再由`ODM`在次基础上进一步精修目标框边界，作者认为这种方法会提升模型整体的精度，尤其是改进对小目标检测的表现

4. `negative anchor ﬁltering`

   负样本筛选，本文的思路是`ARM`将负样本置信度大于门限值 $$\theta$$ 的目标框筛去，$$\theta​$$ 的经验值是`0.99`。也就是说ARM仅将正样本和困难的负样本送进`ODM`进行进一步检测

   困难负样本挖掘采用了与`SSD`一致的方法，将负：正样本比例保持在`3:1`

5. 损失函数

   RefineDet的损失函数由两部分组成，`ACM`和`ODM`，每一部分都包含分类与回归两个损失函数，所以总得损失函数为：

   ![RefineDet_loss](/img/post/RefineDet_loss.jpg)

   $$L_b$$代表二分类的交叉熵损失函数，$$L_m$$代表多分类的`softmax`损失函数，$$L_r$$代表目标框回归的`smooth L1 loss`

## 开源代码

[**RefineDet**](https://github.com/sfzhang15/RefineDet)

## 参考文献

[Single-Shot Refinement Neural Network for Object Detection](https://arxiv.org/pdf/1711.06897.pdf)

