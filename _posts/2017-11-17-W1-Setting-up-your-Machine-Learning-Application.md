---
layout:     post
title:      "Hyperparameter tuning, Regularization and Optimization 第一周笔记"
subtitle:   "Practical aspects of Deep Learning"
date:       2017-11-17 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 公开课
---

# Setting up your Machine Learning Application

## Bias/Variance trade-off

Bias对应训练集误差,Variance对应验证集误差,举例如下

|                 | high variance | high bias | high bias&high variance | low bias&low variance |
| --------------- | ------------- | --------- | ----------------------- | --------------------- |
| Train set error | 1%            | 15%       | 15%                     | 0.5%                  |
| Dev set error   | 11%           | 16%       | 30%                     | 1%                    |

## Regularzation

### L2 Regularization

L2正则化的方法是在成本函数$J$上加一个正则化参数,对这个整体进行优化.也称为"weight decay",相比原来梯度下降的速度更快

$$J(w,b) = \frac{1}{m}\sum_{i=1}^mL(\hat y^{(i)},y^{(i)}) + \frac{\lambda}{2m}||w||^2_2$$

式中 
$$||w||^2_2 = \sum_{j=1}^{n_x}w_j^2 = \sum_{i=1}^{n^{[l]}}\sum_{j=1}^{n^{[l-1]}}(w_{ij}^{[l]})^2= W^TW$$

L1正则化的正则化参数是
$$\frac{\lambda}{2m}\sum_{j=1}^{n_x}|w_j|$$

### Dropout Regularization

dropout在计算机视觉领域应用较多,防止过拟合.dropout的一大缺点是成本函数$$J$$没有明确的定义,所以在调试的时候通常把dropout函数关闭

"Inverted dropout" dropout with layer l=3 keep_prob = 0.8

```python
import numpy as np
d3 = np.rando.rand(a3.shape[0],a3.shape[1])<keep_prob
a3 = np.multiply(a3,d3)
a3 /= keep_prob
```

### Early stopping

在验证集误差开始增大的时候就停止训练,尽管训练误差仍然在减小

### 正则化输入

通过数据预处理把输入值在各个维度都处理为均值为0,方差为1的标准化输入,可以让成本函数$J$更好优化.

均值归0:

$$\mu = \frac{1}{m}\sum_{i=1}^mx^{(i)}$$

$$x := x -\mu$$

方差缩为1:

$$\sigma^2 = \frac{1}{m}\sum_{i=1}^mx^{(i)}**2$$

$$x /= \sigma^2$$

## 梯度消失和梯度爆炸

$$W^{[l]} > I$$在深度神经网络中将引起梯度爆炸,$$W^{[l]} < I $$将引起梯度消失

## 深度神经网络中的权重初始化

$$Z = W_1X_1 + W_2X_2 + ... + W_nX_n$$

为了防止$$Z$$过大可以设置$$W_i = \frac{1}{n}$$

```python
W^[l] = np.random.randn(shape)*np.sqrt(2/n^[l-1])#He 初始化
```

## 梯度的估算与检查

梯度估算中双侧峰值比单侧峰值估算更准确

### 梯度检查

**Instructions**:

- First compute "gradapprox" using the formula above (1) and a small value of $\varepsilon$. Here are the Steps to follow:
    1. $$\theta^{+} = \theta + \varepsilon$$
    2. $$\theta^{-} = \theta - \varepsilon$$
    3. $$J^{+} = J(\theta^{+})$$
    4. $$J^{-} = J(\theta^{-})$$
    5. $$gradapprox = \frac{J^{+} - J^{-}}{2  \varepsilon}$$
- Then compute the gradient using backward propagation, and store the result in a variable "grad"
- Finally, compute the relative difference between "gradapprox" and the "grad" using the following formula:
  $$ difference = \frac {\mid\mid grad - gradapprox \mid\mid_2}{\mid\mid grad \mid\mid_2 + \mid\mid gradapprox \mid\mid_2} \tag{2}$$
  You will need 3 Steps to compute this formula:
   - 1'. compute the numerator using np.linalg.norm(...)
   - 2'. compute the denominator. You will need to call np.linalg.norm(...) twice.
   - 3'. divide them.
- If this difference is small (say less than $10^{-7}$), you can be quite confident that you have computed your gradient correctly. Otherwise, there may be a mistake in the gradient computation. 