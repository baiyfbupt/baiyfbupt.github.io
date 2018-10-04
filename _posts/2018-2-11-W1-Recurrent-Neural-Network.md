---
layout:     post
title:      "Sequence Models 第一周笔记"
subtitle:   "Recurrent Neural Network"
date:       2018-2-11 16:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Course
---
# Recurrent Neural Network

为了处理一句话的序列，首先面临的问题是怎么表示词word？

## Representing word

表示word的一种方式是用字典的方式把每一个词word映射到一个唯一的向量vector，vector可以采用one-hot编码或者其他编码方式

![representing words](\img\post\representing_words.jpg)

## Recurrent Neural Network

有一些**循环神经网络RNN**模型就可以完成上面所说的工作，把词word映射到向量vector

为什么不用全连接的DNN？

- 对于不同数据输入输出的长度都不一样，会带来很多padding的麻烦
- 基础的神经网络不能让文本中的词汇共享参数，会带来大量的参数量和计算量

![RNN](\img\post\RNN.jpg)

\\[a^{<1>}=g_1(W_{aa}a^{<0>}+W_{ax}x^{<1>}+b_a)\\]
g1 represents tanh/relu

\\[\hat y ^{<1>}=g_2(W_{ya}a^{<1>}+b_y)\\]                       
g2 represents sigmoid/softmax

$$a^{<t>}=g_1(W_{aa}a^{<t-1>}+W_{ax}x^{<t>}+b_a)$$

$$\hat y ^{<t>}=g_2(W_{ya}a^{<t>}+b_y)$$                    

经过简化之后，上述前向过程可以写为：

$$a^{<t>}=g_1(W_{a}[a^{<t-1>},x^{<t>}]+b_a)$$

$$\hat y ^{<t>}=g_2(W_{y}a^{<t>}+b_y)$$  

## Backpropagation through time

![BPTT](\img\post\BPTT.jpg)

## Different types of RNNS

1. many to many architecture

![many to many](\img\post\many_to_many.png)

2. many to one architecture

![many to one](\img\post\many_to_one.png)

3. one to one architecture
4. one to many architecture
5. attention architecture

## Language model and sequence generation

Training set：large corpus of english text

在每一句后插入<EOS>标志句子的结尾，用UNK表示不在词典中的词

![language model](\img\post\language_model.jpg)

不断输入Training set的word，输出词典中所有词的后验概率；损失函数是输出结果与label的交叉熵

## Sampling novel sequences

* character level language model:不存在UNK的情况，但是序列更长，更难训练
* word level language model

## Vanishing gradients with RNNS

对于有长期依赖的句子，后面的输出可能与很早以前的输入有关。在求较长序列的梯度时可能存在梯度消失的问题

梯度消失问题比梯度爆炸问题更难解决

## Gated Recurrent Unit(GRU)

![GRU](\img\post\GRU.jpg)

$$c^{<t>}=a^{<t>}$$

$$\hat c^{<t>}=tanh(w_c[\Gamma_r*c^{<t-1>},x^{<t>}]+b_c)$$

$$\Gamma_u = sigmoid(w_u[c^{<t-1>},x^{<t>}]+b_u)$$

$$\Gamma_r=sigmoid(w_r[c^{<t-1>},x^{<t>}]+b_c)$$

$$c^{<t>}=\Gamma_u*\hat c^{<t>}+(1-\Gamma_u)*c^{<t-1>}$$

$$a^{<t>}=c^{<t>}$$

## Long Short Term Memory(LSTM)

![LSTM](\img\post\LSTM.jpg)

$$\hat c^{<t>}=tanh(w_c[a^{<t-1>},x^{<t>}]+b_c)$$

$$\Gamma_u=sigmoid(w_u[a^{<t-1>},x^{<t>}]+b_u)$$

$$\Gamma_f=sigmoid(w_f[a^{<t-1>},x^{<t>}]+b_f)$$

$$\Gamma_o=sigmoid(w_o[a^{<t-1>},x^{<t>}]+b_o)$$

$$c^{<t>} = \Gamma_u*\hat c^{<t>}+\Gamma_f*c^{<t-1>}$$

$$a^{<t>}=\Gamma _o*tanh(c^{<t>})$$

## Bidirectional RNN

BRNN是对传统RNN,GRU,LSTM的一种变化用法，可以让以上模型综合考虑过去和未来的序列对当前信息做出预测

![BRNN](\img\post\BRNN.jpg)

