---
layout:     post
title:      "Densely Connected Convolutional Networks"
subtitle:   "论文阅读简记"
date:       2018-12-4 14:00:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers
---
# Densely Connected Convolutional Networks 论文阅读简记

## 写在前面

这篇文章是CVPR2017的最佳论文，文中提出DenseNet相比于ResNet不仅参数量更少，而且性能也更好。

文中提出的Dense Connection,作者将它的好处总结为：**缓解了梯度消失的问题，加强了特征传播和特征复用，并且可观的减少了参数量**

## 主要思想

文章首先提到了之前已经有很多的方法尝试解决梯度消失的问题：ResNet，High Networks, FractalNets，他们的共同点都是在不同层之间建立一个捷径（short paths）

DenseNet的独特性在于，它在所有特征图（尺寸一样）之间建立了直接连接（Concat），因此第L层就有L个特征图的直接连接，前L层总共有$$\frac{L(L+1)}2$$条连接，相比于传统网络的L条，可以用『密集』来描述了。这种密集连接也被发现有一定的正则化效果。

## 网络结构

一. 网络结构概述

![DenseNet_arch](\img\post\DenseNet_arch.png)

Figure 2表示的是DenseNet的完整结构图，图中的网络包含3个Dense Block，作者将DenseNet分为多个block目的是，保证一个block内特征图的尺寸一致，方便concatenation。在block外部通过Pooling改变特征图的尺寸。

![Dense_block](\img\post\Dense_block.png)

DenseNet的另一个特点就是网络更窄(feature map数量小于100)，参数更少。这主要得益于Figure 1这种dense block的设计。因为每一层都在有原始输入的直接连接，等于有一种内在的约束，更容易训练。DenseNet与ResNet的区别可以通过两个公式直接看出来：

**ResNet:**

$$x_l = H_l(x_{l-1}) + x_{l-1}$$

这里$$l$$表示层，$$x_l$$是第$$l$$层的输入，$$Hl()$$表示非线性变换，作者提到ResNet这里的相加也许不利于网络中的信息流动。

**DenseNet:**

$$x_l=H_l([x_0,x_1,...,x_{l-1}])$$

不同于ResNet,DenseNet在这里先把$$0$$到$$l-1$$层的特征图都做concatenation，然后再做非线性变换(BN, Relu, Conv)

![DenseNet_arch_table](\img\post\DenseNet_arch_table.png)

Table 1介绍了整个网络具体的结构，DenseNet也引入了与ResNet中的bottleneck结构，在每一个3*3卷积前做一个1\*1卷积来降维和减少参数量，DenseNet-BC代表既有bottleneck layer，又有Translation layer.

二. 一些新的概念

1. 密集连接(Dense Connections)

2. 过渡层(Transition Layer)

   过渡层包含瓶颈层(Bottleneck Layer, 即1x1卷积层)和池化层

3. 增长率(Groth rate)

   增长率代表的是每一层输出的feature map数，ResNet, GoogLeNet等结构常常达到上千个，而DenseNet因其高效的特征复用,k可以被限制在一个很小的范围内(小于100)

4. 压缩(Compression)

   跟1x1卷积层作用类似，本文坐着选了压缩率$$\theta$$为0.5,包含Bottleneck Layer的叫DenseNet-B，包含压缩层的叫DenseNet-C，两者都包含的叫DenseNet-BC.

## 开源代码

作者公开了DenseNet的代码，[DenseNet](https://github.com/liuzhuang13/DenseNet)

## 参考文献

1. [Densely Connected Convolutional Networks](https://arxiv.org/pdf/1608.06993.pdf)

2. [CVPR 2017最佳论文作者解读：DenseNet 的“what”、“why”和“how”｜CVPR 2017](https://www.leiphone.com/news/201708/0MNOwwfvWiAu43WO.html)

3. [DenseNet算法详解](https://blog.csdn.net/u014380165/article/details/75142664/)