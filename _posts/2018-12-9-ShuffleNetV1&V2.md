---
layout:     post
title:      "轻量级卷积神经网络(三)：ShuffleNet V1 & V2"
subtitle:   "论文阅读笔记"
date:       2018-12-9 15:31:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers


---

# ShufﬂeNet: An Extremely Efﬁcient Convolutional Neural Network for Mobile Devices

这是Face++的一篇研究轻量级CNN基础模型的论文，与MobileNet V1同样发表于CVPR-2017，但比MobileNet晚两个月在arxiv公开，性能方面在ImageNet数据集上top-1准确率比MobileNet高了7.8%

## 主要思想

ShuffleNet主要包含两个新型的结构：**分组逐点卷积(pointwise group conv)和通道重排(channel shuffle)**。

其中**分组逐点卷积**通过将卷积运算的输入限制在每个组内（[分组卷积](https://blog.csdn.net/hhy_csdn/article/details/80030468)），减少了模型的计算量，但这样带来的新的问题：多层逐点卷积堆叠时，模型的信息流被分割在了各个组内，没有相互之间的信息交换，这会引起模型的表示能力和识别精度。论文用一张彩色图阐述了这个问题

![gconv](/img/post/gconv.png)

所以，在使用分组逐点卷积的同时，需要引入组间信息的交换机制。所以可以考虑打乱各个组的信息排布，让每个卷积核接受的输入由之前各组特征的组合构成。作者提出了这种**通道重排**的方法，如Figure  1(b),(c)。并且文章提到了，由于通道重排列操作也是可导的，所以不会影响网络端到端的训练

## 网络结构

1. **ShuffleNet单元**

   ShuffleNet单元以分组逐点卷积核通道重排为核心，同时引入了ResNet的shortcuts。除了这些之外，为了减少计算量有一些特殊的优化：把原先的两个1x1卷积分组化，变成GConv；使用逐通道3x3卷积代替原来普通的3x3卷积；在第一个1x1卷积后进行通道重排

   需要注意的是，文章提到在3x3DWConv后不再用RELU激活函数了，Xception提到过这个问题

   ![shuffle_units](/img/post/shuffle_units.png)

2. **整体结构**

   文章给出了在ImageNet数据集下模型的配置

   借助 ShuffleNet 结构单元，作者构建了完整的 ShuffeNet 网络模型。**它主要由 16 个 ShuffleNet 结构单元堆叠而成，分属网络的三个阶段，每经过一个阶段特征图的空间尺寸减半，而通道数翻倍。整个模型的总计算量约为 140 MFLOPs。**通过简单地将各层通道数进行放缩，可以得到其他任意复杂度的模型。

   ![shufflenet_arch](/img/post/shufflenet_arch.png)

# ShuﬄeNet V2: Practical Guidelines for Eﬃcient CNN Architecture Design

这篇文章发表在ECCV-2018，同样来自Face++

文章提出了轻量级网络的评价指标不应该只看FLOPs，并且给出了几个轻量级网络设计的准则，并就V1的结构做了改进

## 主要思想

1. **仅用FLOPs衡量网络复杂度的不合理性**

   传统的FLOPs来衡量不能完全衡量一个网络的复杂性，因为有很多其他因素也在影响着最终的速度(更快的速度：这也是我们最直接的目标，文中称为direct metrics)

   速度会被很多其他因素影响："One such factor is memory access cost (MAC). Such cost constitutes a large portion of runtime in certain operations like group convolution. It could be bottleneck on devices with strong computing power, e.g., GPUs. This cost should not be simply ignored during network architecture design. Another one is degree of parallelism. A model with high degree of parallelism could be much faster than another one with low degree of parallelism, under the same FLOPs."

   总结下来：1.访存速度；2.算法的并行性；3.另外还与实际运行的平台有关

2. **轻量级网络设计的指导性意见**

   - 每一个卷积block输入输出的channel应保持一致(考虑访存)
   - 过度的分组卷积会增加访存时间
   - 网络结构碎片化会影响整体的并行性
   - 逐点操作的时间消耗是不可忽略的

## 网络结构

1. **模块设计**

   ![shufflev2_units](/img/post/shufflev2_units.png)

   ShuffleNet v2弃用了1x1的group conv，用1x1普通卷积代替(处于分组卷积访存消耗过大的考虑)。V2提出了一种ChannelSplit操作，将输入channels(假设总共c个)分为两部分，一部分(c1)做identity map，另外一部分(c-c1)做卷积运算。到了模块的末尾，直接将两分支上的输出concat起来，从而规避了原来ShuffleNet v1中Element-wise sum的操作。最后对输出的feature maps进行RandomShuffle操作，促进各channel之间的信息cross-talk。

   跟v1一样，它也提供了一种用于downsampling的模块。为了保证在下采样的时候增加整体输出channels数目，它取消了模块最开始时的RandomSplit操作，从而分别操作最后再拼结，使得最终outptu channels数目实现翻倍。

2. **整体架构**

   ![shufflev2_arch](/img/post/shufflev2_arch.png)

   可以看到，v2与v1的整体结构区别不大，主要的区别在每个unit内部，类似于mobilenet，它也引入了一个超参数s通过控制输出channel数来控制模型的尺寸

论文最后还将ShuffleNetV2与DenseNet做了对比,认为V2也有类似于DenseNet的特征重用："Thus, the structure of ShuﬄeNet V2 realizes this type of feature re-use pattern by design. It shares the similar beneﬁt of feature re-use for high accuracy as in DenseNet [6], but it is much more eﬃcient as analyzed earlier."

# 开源代码

作者本身没有开源代码，可以参考Github上的其他实现：[V1 in tensorflow](https://github.com/MG2033/ShuffleNet), [V2 in pytorch and caffe](https://github.com/miaow1988/ShuffleNet_V2_pytorch_caffe)

# 参考文献

1. [ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile
   Devices](https://arxiv.org/pdf/1707.01083.pdf)
2. [ShuﬄeNet V2: Practical Guidelines for Eﬃcient CNN Architecture Design](https://arxiv.org/pdf/1807.11164.pdf)
3. [为移动 AI 而生——旷视(Face++)最新成果 ShuffleNet 全面解读](https://www.sohu.com/a/156321743_418390)
4. [精简CNN模型系列之六：ShuffleNet v2](https://www.jianshu.com/p/71e32918ea0a?utm_source=oschina-app)