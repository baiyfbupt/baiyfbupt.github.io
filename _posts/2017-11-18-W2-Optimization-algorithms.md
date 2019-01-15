---
layout:     post
title:      "Hyperparameter tuning, Regularization and Optimization 第二周笔记"
subtitle:   "Optimization algorithms"
date:       2017-11-18 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 公开课
---

# Optimization algorithms

## Mini-batch

把整个训练集batch分为多个子训练集mini-batch,mini-batch训练速度远高于batch

a epoch 代表遍历了一遍整个训练集

if mini-batch size = m : batch gradient descent $$(X^{\{1\}},Y^{\{1\}}) = (X,Y)$$ #训练时间太长

if mini-batch size = 1 : Stochastic Gradient Descent(SGD)每一个样本都是它自己的mini-batch#失去了所有向量化带来的加速

所以需要选择一个(1,m)之间适中的mini-batch

**Some Guidelines:**

| Small training set(<=200) | use batch gradient descent           |
| ------------------------- | ------------------------------------ |
| **Typical training set**  | **mini-batch = 64,128,256,512,1024** |

## Exponentially weighted averages

$$[V_t = \beta V_{t-1} + (1 - \beta)\theta _t = 0.1\theta_t + 0.1\beta^{1}\theta_{t-1} +0.1\beta ^2\theta_{t-2}+\cdots +0.1\beta^n\theta_{t-n}$$

\\[V_t \approx \frac{1}{1-\beta}days\\]

### 指数加权平均的偏差修正

\\[V_t = \frac{V_t}{1-\beta^t}\\]

## Momentum(Gradient Descent with Momentum)

基本思想是**计算梯度的指数加权平均数**,并且用这个平均数更新梯度

```python
Vdw = beta*Vdw + (1-beta)dw
Vdb = beta*Vdb + (1-beta)db#用Vdw,Vdb更新参数
W = W - learning_rate*Vdw
b = b - learning_rate*Vdb
```

## RMSprop

```python
Sdw = beta_2*Sdw + (1-beta_2)dw^2
Sdb = beta_2*Sdb + (1-beta_2)db^2
W = W - learning_rate*dw/np.sqrt(Sdw)
b = b - learning_rate*db/np.sqrt(Sdb)
```

## Adam optimization algorithm

Adam结合了Momentum和RMSprop

\\[Vdw = 0,Sdw = 0, Vdb = 0,Sdb =0\\]

on iteration t:

$$Vdw = \beta_1Vdw + (1-\beta_1)dw,Vdb = \beta_1Vdb + (1-\beta_1)db$$

$$Sdw = \beta_2Sdw + (1-\beta_2)dw^2,Sdb = \beta_2Sdb + (1-\beta_2)db^2$$

$$V_{dw}^{corrected} = V_{dw}/(1-\beta_1^t),V_{db}^{corrected} = V_{db}/(1-\beta_1^t)$$

$$S_{dw}^{corrected} = S_{dw}/(1-\beta_2^t),S_{db}^{corrected} = S_{db}/(1-\beta_2^t)$$

$$W = W - \alpha\frac{V_{dw}^{corrected}}{\sqrt{S_{dw}^{corrected}}+\varepsilon},b = b - \alpha\frac{V_{db}^{corrected}}{\sqrt{S_{db}^{corrected}}+\varepsilon}$$

## 学习率衰减

### 指数衰减

$$\alpha = \beta^{epoch\_num}*\alpha_0$$

