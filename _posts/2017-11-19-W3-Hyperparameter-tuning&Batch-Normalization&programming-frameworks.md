---
layout:     post
title:      "Hyperparameter tuning, Regularization and Optimization 第三周笔记"
subtitle:   "Hyperparameter tuning&Batch Normalization&Programming Frameworks"
date:       2017-11-19 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Machine Learning
    - Course
---

# Hyperparameter tuning&Batch Normalization&Programming Frameworks

## Hyperparameters

在深度神经网络中有很多的超参数,例如$$\alpha,\beta,\beta_1,\beta_2,\epsilon,layers\_l,hidden\_units,learning\_rate\_decay$$等等

### Appropriate scale for hyperparameters

主要思想是对范围在[a,b]区间上的超参数,在$$[np.log(a),np.log(b)]$$上均匀取值

### 训练实践

Panda**(Babysitting one model)** VS Caviar**(Training many models in parallel)**

## Batch Normalization

### Implementing Batch Norm

对神经网络中的$$Z^{(i)}$$做归一化：

$$\mu = \frac{1}{m}\sum_iZ^{(i)}$$

$$\sigma^2 = \frac{1}{m}(Z_i - \mu)^2$$

$$Z^{(i)}_{norm} = \frac{Z^{(i)} - \mu}{\sqrt{\sigma^2 + \epsilon}}$$

$$\breve Z^{[i]} = \gamma Z^{[i]}_{norm} + \beta$$

### 测试集的Batch Normalization
用训练集上统计的滑动平均值来做测试过程的BN

## Softmax Regression

Softmax用于处理多分类问题，在神经网络的最后一层部署,主要是非线性函数有所不同

$$Z^{[l]} = W^{[l]}A^{[l-1]} + b^{[l]}$$

$$A^{[l]} = \frac{e^{Z^{[l]}}}{\sum e^{Z^{[l]}}}$$

$$dZ^{[l]} = \hat y -y$$


