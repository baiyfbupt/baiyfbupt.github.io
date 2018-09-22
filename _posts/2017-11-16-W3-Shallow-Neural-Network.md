---
layout:     post
title:      "Neural Networks and Deep Learning 第三周笔记"
subtitle:   "Shallow Neural Networks"
date:       2017-11-16 12:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Machine Learning
    - Course
---

# Shallow Neural Network

## 前向传播

\\[Z^{[1]} = w^{[1]}X + b^{[1]}\\]

\\[A^{[1]} = g^{[1]}(Z^{[1]})\\]

\\[Z^{[2]} = W^{[2]}A^{[1]} + b^{[2]}\\]

\\[A^{[2]} = g^{[2]}(Z^{[2]})\\]

## 反向传播

\\[dA^{[2]} = -\frac{Y}{A^{[2]}} + \frac{1-Y}{1-A^{[2]}}\\]

\\[dZ^{[2]} = A^{[2]} - Y\\]

\\[dw^{[2]} = \frac{1}{m}dZ^{[2]}A^{[1]T}\\]

\\[db^{[2]} = \frac{1}{m}np.sum(dZ^{[2]},axis = 1,keepdims = True)\\]

\\[dZ^{[1]} = w^{[2]T}dZ^{[2]} * g^{[1]'}(Z^{[1]})​\\]

\\[dw^{[1]} = \frac{1}{m}dZ^{[1]}X^{T}\\]

\\[db^{[1]} = \frac{1}{m}np.sum(dZ^{[1]},axis = 1,keepdims = True)\\]

## 初始化参数

　　神经网络的W参数不能初始化为0,否则所有隐藏单元都是対称,在梯度下降时所有节点仍保持相等

　　正确的初始化方法是随机初始化

```python
w = np.random.rand(x,y)*0.01#小的参数更有利与激活函数计算(若激活函数是sigmoid或tanh)
b = np.zeros(shape = (x,1))
```

