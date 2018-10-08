---
layout:     post
title:      An Introduction of Person Re-identification
subtitle:   draft version
date:       2018-10-08
author:     Ed He
catalog: true
tags:
    - Machine Learning
    - Person re-ID
---

做了一段时间的person re-identification，一直想写个综述，趁现在还记得，赶紧写点东西。person re-ID中文名可以叫做行人重识别，近几年在CV各大会议上都是十分火爆，而且随着deep learning在这个领域的运用越来越成熟，其识别率也越来越高，虽然在实际应用中还会有各种各样的问题，但最起码我们已经可以看到这个领域的逐渐成熟。我会根据我自己的理解对这个领域做个简单介绍。详细介绍还得等我的毕业论文完成批改，然后会以英文版放出。

## Introduction
如果只考虑image-based person re-ID的话，用一句简单的话来说，person re-ID aims to match the images from non-overlapping cameras or **one camera at different time**。前面一种情况就是假设我们现在有一片宽广的场地，里面有两个摄像头，每个在这个场地里经过的人都会被这两个摄像头拍到，我们便能得到两组图片，分别来自于两个摄像头，那么re-ID问题其实并不关注每个人是谁，它主要是想根据其中一个摄像头的图片，来找到另一个摄像头中的与其对应的图片，这里的摄像头数量也可以不止是两个；后面的一种情况更多出现在大门的门禁系统中，行人在不同的时间经过同一个摄像头，我们同样要根据一个时刻的图片来进行匹配。后面这种情况其实较少被研究，那些public datasets也主要是研究前一种情况。

除了image based的方法之外，还有video based person re-ID，这个领域我没有进行过具体研究，如果想要了解更多，建议阅读Liang Zheng *et al.* 的[Person Re-identification: Past, Present and Future](https://arxiv.org/abs/1610.02984)。

## Motivations
说motivation那其实就是说其应用。person re-ID的应用可以说是比较广泛的，因为其主要关注不同camera获得的图片之间的匹配，各种安防系统，大型监控系统，都可以运用此技术。

## Methodology
在具体进行研究的时候，往往我们要做的事情是根据一张或多张query image来对gallery images进行rank。一张query的情况是single query，多张query的情况就是multi query。而这里的根据主要是各个图片之间的相似度，具体应用时可以使用图片的feature之间的欧式距离，亦或是学习一个距离度量（XQDA and so on)。也就是：
1. feature extration
2. metric learning (or use some existed metrics)
3. distance measuring and ranking

我画了一张更为详细的流程图：
![procedure](/img/procedure.jpg)
这个流程图里融合了一些基本特征，来获得一个更为robust的特征，在实际应用中也是十分常见。
然后这里也就可以引出re-ID的两个主要研究方向：
1. The choice of feature
2. Metric Learning

第一个研究方向主要是寻找对于光照，尺度等外界干扰因素不敏感的特征描述符，第二个研究方向则是学习一个合适的距离度量，来衡量图片之间的相似度。值得一提的是，随着deep learning方法的逐渐成熟，metric learning的重要性已经不复当年，基本现在的re-ID都是先用一个baseline neural network model(resnet比较常用)去提取图片的feature，然后直接用欧式距离进行rank，metric learning能给这种模型带来的提升十分有限。

除了这两种基本方法之外，还有很多的优化方法被提了出来，诸如利用GAN进行data augmentation，re-ranking，迁移学习等，都具有深入研究的价值。

另外，前面假设我们用的都是普通RGB摄像头来采集数据，如果我们用RGB-D摄像头（如kinect）来采集数据的话，除了基本的RGB信息外，我们还可以获得图像的深度信息，那么就可以建立图像的3D点云图，它相比于普通的RGB信息的优点就是更加不容易受光照影响，即使在无光照条件下也能工作，但是考虑到这种摄像头一般只能作用于室内，因此无法大范围应用。

**未完待续**
