---
layout:     post
title:      1x1 Convolution in Neural Network
subtitle:   How it works
date:       2018-10-10
author:     Ed He
catalog: true
tags:
    - Deep Learning
    - CNN
---

最近在阅读[Network in Network](https://arxiv.org/pdf/1312.4400.pdf)和[GoogLeNet](https://arxiv.org/pdf/1409.4842.pdf)原文时，发现两篇文章都重点介绍了1x1卷积的使用。
其实1x1卷积操作与普通的卷积没什么区别，只是卷积核大小为1x1，它的主要作用有：
1. 降维和升维（参考ResNet）；
2. 在不改变feature map大小的基础上增加非线性，增加模型深度以提高模型的表达能力。

On the other hand，1x1卷积就是在对各个通道的feature map进行线性叠加，根据输出通道数量的不同达到升维和降维的目的。降维可以减少模型的参数（参考GoogLeNet）。另外，因为是通道之间的线性叠加，也实现了跨通道信息交互。也就是说，如果1x1的卷积作用于单通道的feature map上，是没有意义的。
