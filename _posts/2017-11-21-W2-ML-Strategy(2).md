---
layout:     post
title:      "Structuring Machine Learning Projects 第二周笔记"
subtitle:   "ML Strategy(2)"
date:       2017-11-21 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Machine Learning
    - Course
---

# ML Strategy(2)

## Error analysis

### Incorrectly labeled

DL算法对随机错误有很强的鲁棒性，所以一些随机误差不会影响训练的结果

而系统性的错误对模型影响较大

### Build your first system quickly then iterate

* Set up dev/test set and metric
* Build initial system quickly
* Use Bias&Variance analysis&Error analysis to prioritize next step
* bias来自模型对训练集数据预测的偏差，variance来自模型对训练集和验证集预测的偏差之差。参考[机器学习老中医：利用学习曲线诊断模型的偏差和方差](https://zhuanlan.zhihu.com/p/33220323)

## 训练集与验证/测试集不匹配

| set                    | error percentage | the bias comes from              |
| ---------------------- | ---------------- | -------------------------------- |
| Human level            | 4%               |                                  |
| Training set error     | 7%               | avoidable bias                   |
| Training-dev set error | 10%              | variance problem                 |
| Dev error              | 12%              | data mismatch                    |
| Test error             | 12%              | degree of overfitting to dev set |

### 数据不匹配问题

在误差分析后确实存在数据不匹配问题，可以人工数据合成造数据

## 迁移学习

通常在迁移问题的数据样本并不足的时候应用，用相关的数据较多的问题A做pre-training，数据较少的问题B做fine-tune

### pre-training

预先初始化，用问题A的数据首先训练模型

### fine-tune

针对B问题重新训练和调整权重，往往只微调最后的一层或者几层的权重

## 多任务学习

训练一个神经网络做多件事往往比训练多个神经网络做多件事效果更好