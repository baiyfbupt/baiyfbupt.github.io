---
layout:     post
title:      "Neural Networks and Deep Learning 第四周笔记"
subtitle:   "Deep Neural Networks"
date:       2017-11-16 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Course
---
# Deep Neural Network

## from logistic regression to neural network

![logistic regression to neural network](/img/post/from_lr_to_dnn.jpg)

## 深度神经网络的参数维度

\\[W^{[l]},dW^{[l]} : (n^{[l]} , n^{[l-1]})\\]

\\[b^{[l]},db^{[l]}: (n^{[l]} , 1)\\]

\\[Z^{[l]} ,dZ^{[l]},A^{[l]},dA^{[l]}: (n^{[l]} , m)\\]

## DNN的前向传播和反向传播

### 前向

\\[Z^{[l]} = W^{[l]}A^{[l-1]} + b^{[l]}\\]

\\[A^{[l]} = g^{[l]}(Z^{[l]})\\]

### 反向

\\[dZ^{[l]} = dA^{[l]} * g^{[l]'}(Z^{[l]})\\]

\\pdW^{[l]} = dZ^{[l]}A^{[l-1]T}\\]

\\[db^{[l]} = \frac{1}{m}np.sum(dZ^{[l]},axis =1,keepdims = True)\\]

\\[dA^{[l-1]} = W^{[l]T}dZ^{[l]}\\]

## 参数和超参数

**参数**是指W,b这些神经网络可以自学习调节的

**超参数**:learning rate,#iteration,#hidden layer L,#hidden units n,choice of activation function在一定程度控制参数,所以称为超参数