---
layout:     post
title:      "轻量级卷积神经网络(一)：MobileNets: V1 & V2"
subtitle:   "论文阅读笔记"
date:       2018-12-7 11:31:00
author:     "baiyf"
header-img: "img/about-bg-walle.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Papers
---

# MobileNets: Efﬁcient Convolutional Neural Networks for Mobile Vision Applications

MobileNets是为移动端和嵌入式设备提出的一种轻量级模型，文章使用了深度分离卷积(depthwise separable convolutions)，并且引入了两个超参数：width multiplier, resolution multiplier，用来在性能和效率之间做折衷

最后论文也在其他领域(目标检测，人脸识别，大规模地理定位等)上验证了模型的有效性

## 主要思想

1. **Depthwise Seperable Convolution**

   文章使用了一种深度分离卷积的layer, 整个模型围绕这个结构展开

   深度分离卷积就是：把标准卷积分割成**深度卷积(depthwise convolution)**和**逐点卷积(pointwise convolution)**, 第一个卷积每个filter都只跟input的每一个channel做卷积, 并将结果concat起来，所以filter个数与M有关；后一个卷积负责combining，用跨channel的1x1卷积把上一部卷积得到的结果融合起来

   这么做的好处是可以节省计算量和减少参数。引用论文中的示例来说明

   ![depwiseconv](/img/post/depwiseconv.png)

   假设输入特征图尺寸为$$D_F*D_F*M$$, 卷积核尺寸为$$D_K*D_K*M*N$$(输出channel数为N的特征图)，那么一次卷积总的计算量为(stride为1)：

   $$D_F*D_F*M*N*D_K*D_K$$

   然而深度分离卷积的总计算量：

   $$D_K*D_K*M*D_F*D_F+M*N*D_F*D_F$$

   深度分离卷积与传统卷积层结构的对比可以参考下图：

   ![depth_arch](/img/post/depth_arch.png)

2. **模型折衷**

文章针对嵌入式设备上对性能和效率折衷的需求，新引入了两个超参数：Width Multiplier和Resolution Multiplier

**Width Multiplier:**对某个层的输入输出都乘以$$\alpha$$，这是一个小于等于1的正数，做了这个缩放以后计算量约为原来的$$\alpha^2$$倍:

$$D_K*D_K*\alpha M*D_F*D_F+\alpha M*\alpha N*D_F*D_F$$

**Resolution Multiplier:**对原始输入图像尺寸做缩放$$\rho$$，同样是一个小于等于1的正数，使用后计算量约为原来的$$\rho^2$$倍:

$$D_K*D_K*M*\rho D_F*\rho D_F+M*N*\rho D_F*\rho D_F$$

最终在ImgeNet上取得了精度只掉1%,但是参数量和计算量大幅度减少的预期结果

![depthwise_compare](/img/post/depthwise_compare.png)

## 网络结构

![mobilenet_arch](/img/post/mobilenet_arch.png)

第一层卷积过后全部用深度分离卷积代替了传统的卷积，最后用了Global Average Pooling接FC，最终送进softmax分类

# MobileNetV2: Inverted Residuals and Linear Bottlenecks 

本文基于MobileNetV1,结合了残差网络的思想提出了一种新的轻量级网络架构，在多个任务上取得了State-of-art的水平

这篇文章提出了一种倒置残差结构(inverted residual structure)，结合了残差网络的优点并有效的减少了模型复杂度；本文另外还对主分支的非线性变换的有效性展开了实验，并证明了这部分去除非线性的必要性

## 主要思想

1. **Linear bottleneck**

   这部分作者从兴趣流行(manifold of interest)的角度阐述了理由，简单理解就是作者认为激活函数在高维空间能有效增加非线性，维度空间会破坏信息流动，不如线性效果好，所以第二个1x1卷积后不再适合用relu

   ![v2_block](/img/post/v2_block.png)

2. **Inverted residuals**

   ![resnet_mobilenet_comp](/img/post/resnet_mobilenet_comp.png)

   不同于ResNet的残差结构，这里的Inverted residuals是中间的更宽，两边的特征图较窄,shortcuts放在linear bottleneck之间,论文中用下图描述了Inverted residual与普通residual的不同

   ![inverted_residual](/img/post/inverted_residual.png)

## 网络结构

![mobilenetv2_arch](/img/post/mobilenetv2_arch.png)

网络架构以普通卷积开始，之后跟了若干个residual bottleneck，激活函数使用了RELU6：$$f(x)=min\{(0,x),6\}$$,在低精度下有更强的鲁棒性

# 开源代码

谷歌在Tensorflow models repo下开源了MobileNet系列的代码: [tensorflow/models](https://github.com/tensorflow/models/tree/master/research/slim/nets/mobilenet)

# 参考文献

1. [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/pdf/1704.04861.pdf)
2. [MobileNetV2: Inverted Residuals and Linear Bottlenecks](https://arxiv.org/pdf/1801.04381.pdf)
3. [轻量级网络--MobileNet论文解读](https://blog.csdn.net/yifen4234/article/details/80163319)