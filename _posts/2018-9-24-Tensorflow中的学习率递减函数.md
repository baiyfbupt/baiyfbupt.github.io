---
layout:     post
title:      "TensorFlow中常用的学习率递减方法"
subtitle:   "TensorFlow learning rate decay"
date:       2018-9-24 16:00:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Deep Learning
    - TensorFlow
---

# Tensorflow中常用的学习率递减方法
在训练神经网络时,控制学习率对训练的速度和准确度都有很大作用.逐渐减小学习率在实践中被证明对训练的收敛有正向效果,Tensorflow自带几种衰减方法:

- tf.train.piecewise_constant　分段常数衰减
- tf.train.inverse_time_decay　反时限衰减
- tf.train.polynomial_decay　多项式衰减
- tf.train.exponential_decay　指数衰减
- tf.train.natural_exp_decay　自然指数衰减
- tf.train.cosine_decay　余弦衰减
- tf.train.linear_cosine_decay　线性余弦衰减
- tf.train.noisy_linear_cosine_decay　噪声线性余弦衰减 

****
## tf.train.piecewise_constant
分段常数衰减就是在定义好的区间上，分别设置好不同的常数学习率值，作为学习率的初始值和后续衰减值

输入：

- 0-D标量Tensor 
- boundaries: 区间，tensor\|list
- values: 不同区间上的学习率值
- name: 操作的名称，默认为PiecewiseConstant

示例：

```python
#!/usr/bin/python
# coding:utf-8

# piecewise_constant 阶梯式下降法
import matplotlib.pyplot as plt
import tensorflow as tf
global_step = tf.Variable(0, name='global_step', trainable=False)
boundaries = [10, 20, 30]
learing_rates = [0.1, 0.07, 0.025, 0.0125]
y = []
N = 40
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    for global_step in range(N):
        learing_rate = tf.train.piecewise_constant(global_step, boundaries=boundaries, values=learing_rates)
        lr = sess.run([learing_rate])
        y.append(lr[0])

x = range(N)
plt.plot(x, y, 'r-', linewidth=2)
plt.title('piecewise_constant')
plt.show()
```


## tf.train.expotional_decay
学习率指数衰减，是最常用的衰减方法
输入：

- learning_rate: 初始学习率
- global_step: 用于计算全局步数，非负，用于逐步计算衰减指数
- decay_steps: 衰减指数，必须是正值，决定衰减周期
- decay_rate: 衰减率
- staircase: 若为True,则以不连续的阶梯状衰减(就是在相同epoch内保持相同的学习率)，若为False, 则是标准指数型衰减
- name: 操作的名称，默认ExponentialDecay

指数衰减学习率计算公式：
```
learning_rate = learning_rate * devay_rate^(global_step / decay_steps)
```

```python
exponential_decay(
    learning_rate,
    global_step,
    decay_steps,
    decay_rate,
    staircase=False,
    name=None
)
```
## tf.train.natrual_exp_decay
natural_exp_decay 和 exponential_decay 形式近似，natural_exp_decay的底数是e．自然指数衰减比指数衰减要快的多，一般用于较快收敛，容易训练的网络

- learning_rate: 初始学习率
- global_step: 全局step数
- decay_steps：学习率衰减的步数,也代表学习率每次更新相隔的步数
- decay_rate: 衰减率
- staircase: 若为True,则以不连续的阶梯状衰减(就是在相同epoch内保持相同的学习率)，若为False, 则是标准型衰减
- name: 操作的名称，默认ExponentialTimeDecay

自然指数衰减的学习率计算公式为：
```python
decayed_learning_rate = learning_rate * exp(-decay_rate * global_step)
```

```python
natural_exp_decay(
    learning_rate,
    global_step,
    decay_steps,
    decay_rate,
    staircase=False,
    name=None
)
```


## tf.train.polynomial_decay
函数使用多项式衰减，在给定的decay_steps将初始学习率帅见到指定的学习率
输入:

- learning_rate：初始值
- global_step：全局step数
- decay_steps：学习率衰减的步数,也代表学习率每次更新相隔的步数
- end_learning_rate：衰减最终值
- power：多项式衰减系数
- cycle：step超出decay_steps之后是否继续循环
- name：操作的名称，默认为PolynomialDecay。


参数cycle目的：防止神经网络训练后期学习率过小导致网络一直在某个局部最小值中振荡；这样，通过增大学习率可以跳出局部极小值．


```python3
decay_steps = decay_steps * ceil(global_step / decay_steps)
decayed_learning_rate = (learning_rate - end_learning_rate) *
                        (1 - global_step / decay_steps) ^ (power) + end_learning_rate
```


```python3
polynomial_decay(
    learning_rate,
    global_step,
    decay_steps,
    end_learning_rate=0.0001,
    power=1.0,
    cycle=False,
    name=None
)
```

## tf.inverse_time_decay
函数应用反向衰减函数提供初始学习速率，利用global_step来计算衰减的学习速率，计算公式为：

```
decayed_learning_rate =learning_rate/(1+decay_rate* global_step/decay_step)
```
当staircase=True时：
```
decayed_learning_rate =learning_rate/(1+decay_rate*floor(global_step/decay_step))
```
输入：

- learning_rate：初始学习率．
- global_step：用于衰减计算的全局步数．
- decay_steps：衰减步数．
- decay_rate：衰减率．
- staircase：是否应用离散阶梯型衰减．（否则为连续型）
- name：操作的名称，默认为InverseTimeDecay．

示例：

```
#!/usr/bin/python
# coding:utf-8

import matplotlib.pyplot as plt
import tensorflow as tf
y = []
z = []
N = 200
global_step = tf.Variable(0, name='global_step', trainable=False)

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    for global_step in range(N):
        # 阶梯型衰减
        learing_rate1 = tf.train.inverse_time_decay(
            learning_rate=0.1, global_step=global_step, decay_steps=20,
            decay_rate=0.2, staircase=True)
        # 连续型衰减
        learing_rate2 = tf.train.inverse_time_decay(
            learning_rate=0.1, global_step=global_step, decay_steps=20,
            decay_rate=0.2, staircase=False)
        lr1 = sess.run([learing_rate1])
        lr2 = sess.run([learing_rate2])

        y.append(lr1[0])
        z.append(lr2[0])

x = range(N)
fig = plt.figure()
ax = fig.add_subplot(111)
plt.plot(x, z, 'r-', linewidth=2)
plt.plot(x, y, 'g-', linewidth=2)
plt.title('inverse_time_decay')
ax.set_xlabel('step')
ax.set_ylabel('learing rate')
plt.show()
```



****
每个函数的返回值都是与learning_rate类型相同的tensor标量值
