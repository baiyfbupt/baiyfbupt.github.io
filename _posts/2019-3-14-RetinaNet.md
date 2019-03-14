---
layout:     post
title:      "Focal loss-解决单阶段目标检测类别不均衡问题的又一利器"
subtitle:   "论文阅读笔记"
date:       2019-3-14 17:00:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers



---

# Focal Loss for Dense Object Detection

## 写在前面

本文由FAIR发表在ICCV-2017上

本文作者认为，现阶段单阶段目标检测器性能稍逊的原因是其前景-背景不均衡的问题尤为严重

为了解决这个类别不均衡的问题，已经有了`OHEM`的方法，但是本文做法认为这种完全忽视容易负样本的做法有所不妥，虽然简单负样本无异于模型迭代优化，但是它的数量众大，加在一起也不可小觑，所以提出了本文改进的思路

本文在`COCO`数据集上以`5fps`的速度达到了`39.1AP`的准确率

## 主要思想

本文将传统的`交叉熵损失函数CE`进行改进，将`简单负样本loss值进行降权`。换言之，就是让模型更专注于困难负样本和正样本，作者在`FPN`的基础上做了这部分改进，loss函数的改进如下图所示

![focal_loss](focal_loss.jpg)

改进之后与现阶段state-of-art算法的性能对比如下图所示：

![RetinaNet](RetinaNet.jpg)

## 开源代码

[**Detectron**](https://github.com/facebookresearch/Detectron)

## 参考文献

[Focal Loss for Dense Object Detection](https://arxiv.org/pdf/1708.02002.pdf)