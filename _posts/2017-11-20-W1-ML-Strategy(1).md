---
layout:     post
title:      "Structuring Machine Learning Projects 第一周笔记"
subtitle:   "ML Strategy(1)"
date:       2017-11-20 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Machine Learning
    - Course
---

# ML Strategy(1)

## Orthogonalization

正交化是指让多个超参数的调整互相不影响

ML的基本要求：

1. 模型在训练集上拟合情况良好（bigger network）
2. 模型在验证集上拟合情况良好(Regularzation,bigger training set)
3. 模型在测试集上拟合情况良好(bigger dev set)
4. 模型在真实环境下表现良好(change dev set or cost function)

对应有相应的正交化超参数调整

## 单一数字评估指标

* 查准率P：对图片识别的准确率
* 查全率R：对类别中所有图片能识别的比例
* F1 Score:$$\frac{2}{\frac1P+\frac1R}$$是查准率和查全率的综合评价，是单一数字评估指标

在系统的性能表现中往往只有一个性能指标是需要不断优化的，其他性能指标只要满足一定阈值就可以1(Optimizing metric),N-1(Satisficing metric)

## Train/Dev/Test set

Dev set and Test set 应来自统一分布

## 与人类表现相比

只要ML的性能表现低于人类就可以：

* 获取更多带标签的数据
* 分析人表现更好的原因
* 分析偏差和方差

可避免偏差（avoidable bias）实际训练偏差与贝叶斯偏差的差值，这个值的存在表明模型存在的优化空间

avoidable bias与Dev error - Training error的大小决定了哪一项有更大的优化空间

