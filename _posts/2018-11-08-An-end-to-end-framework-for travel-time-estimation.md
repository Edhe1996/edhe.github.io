---
layout:     post
title:      An end-to-end framework for travel time estimation - DeepTTE
subtitle:   When Will You Arrive? Estimating Travel Time Based on Deep Neural Networks
date:       2018-11-08
author:     Ed He
catalog: true
tags:
    - Deep Learning
    - ETA
    - RNN
---

### DeepTTE

DeepTTE is comprised of three components: attribute component, spatio-temporal learning component, and multi-task learning component. The attribute component is used to processes the external factors (e.g. weather) and the basic information of the given path (e.g. start time). Its output is fed to the other two components as a part of their inputs. The spatio-temporal learning component is the main building component that learns the spatial correlations and temporal dependencies from the raw GPS location sequences. Finally, the multi-task learning component estimates the travel time of the given path based on the previous two components. It is capable of balancing the tradeoff between individual estimation and collective estimation with a multi-task loss function.

![architecture](https://ws3.sinaimg.cn/large/006tNbRwgy1fwzagebizdj319c0ve49h.jpg)

#### Attribute component
This component mainly deals with many complex factors, including the weather condition, the driving habits, the day of week and the start time of the travel. They are represented as *weatherID*, *driverID*, *weekID* and *timeID* respectively. Embedding method is utilized to transform these categorical attributes into low-dimensional real vectors. The *travel distance* is also concatenated with other attributes and the output vector is denoted as *attr*.

#### Spatio-temporal component
The spatio-temporal component consists of two parts. The first part is a **geo-convolutional neural network** which transforms the raw GPS sequence to a series of feature maps. Such component captures the local spatial correlation between consecutive GPS points. The second part is the **recurrent neural network** which learns the temporal correlations of the obtained feature maps.

It is worth mentioning that every GPS location point in the historical trajectory *T* contains the corresponding longitude/latitude and the timestamp.

For each GPS point `Pi` in the sequence, a non-linear mapping is applied:

![formular1](https://ws3.sinaimg.cn/large/006tNbRwly1fwzdrlkpb7j30j602c74h.jpg)

```math
loc_{i}^{conv} = \sigma (W_{conv} * loc_{i:i+k-1} + b)
```

The feature is then concatenated with all the local path distances to form the compact feature map `loc^f`.

![geo_conv](https://ws4.sinaimg.cn/large/006tNbRwly1fwzdttv6raj30rq0fgacb.jpg)

The feature map `loc^f` captures the spatial dependencies of all the local paths. To further capture the temporal dependencies among these local paths, RNN is applied. The feature map `loc^f` can be regarded as a sequenceof spatial features with length `|T| − k + 1`. Furthermore, the attributes information (*attr*) is incorporated into the feature maps as the input of the RNN. Thus, the updating rule of the recurrent layer can be expressed as:

```math
h_i = \sigma_{rnn} (W_x~\cdot~loc_i^f + W_h~\cdot~h_{i-1} + W_a~\cdot~attr)
```

Two stacked LSTM layers are used to overcome vanish gradient problem while processing long sequences. It has been shown that a stacked LSTM is more efficient to increase the model capacity compared with a single layer LSTM.

#### Multi-task learning component
This component combines the previous components and estimates the travel time of input path. As mentioned before, the *individual estimation* and the *collective estimation* are combined in the proposed model. During the training phase, they enforce the multi-task learning component to accurately estimate the travel time of both entire path and each local path simultaneously. During the test phase, they eliminate the local path estimation part and report the estimated travel time of the entire path.

* **Estimate the local paths**: Use two stacked fully-connected layers to map each `h_i` in the sequence obtained from spatio-temporal component  to a scalar `r_i`, which represents the travel time of the `i-th` local path.

* **Estimate the entire path**: Attention mechaniam is adopted to estimate the travel time of the entire path:

```math
h_{att} = \sum_{i = 1}^{|T|-k+1}a_i~\cdot~h_i
```

where `a_i` is the weight for the `i-th` path. The vector *attr* in the attribute component captures the effect of external factors, and the feature sequence `{h_i}` captures the spatio-temporal features of local paths. Thus:

```math
z_i = <\sigma_{att}(attr), h_i>,~a_i = \frac{e^{z_i}}{\sum_j e^{z_j}}
```

where `⟨.⟩` is the inner product operator and `σ_att` is a non-linear mapping which maps *attr* to a vector with the same length as `h_i`. 

Finally, the attention based vector `h_att` is passed to residual fully-connected blocks to obtain the estimation of the entire path, which is donoted as `r_en`.


### Model training
The *mean absolute precent error* (MAPE) is used as the objective function. Multiple criterions are used to evaluate the model, including rooted mean squared error (RMSE) and the mean absolute error (MAE).

For the local path estimation, they define the corresponding loss as the average loss of all local paths: 

```math
L_{local} = \frac{1}{|T|-k+1} \sum_{i=1}^{|T|-k+1}|\frac{r_i - (p_{i+k-1}.ts - p_i.ts)}{p_{i+k-1}.ts - p_i.ts + \epsilon}|
```

For the entire path, the corresponding loss is defined as:

```math
L_{en} = \frac{|r_{en} - (p_{|T|}.ts - p_1.ts)|}{p_{|T|}.ts - p_1.ts}
```

The proposed model is trained to mininized the weighted combination of two loss terms:

```math
\beta~\cdot~L_{local} + (1 - \beta)~\cdot~L_{en}
```

In this paper, β is fixed as 0.3. During the test phase, the travel time estimation of the entire path `r_en` is regarded as the final estimation.

### Experiment results
The proposed model is tested on two large scale real-world datasets: **Chengdu dataset** and **Beijing dataset**. It is reported that the proposed model gets state-of-the-art performance on both datasets.

![performance](https://ws4.sinaimg.cn/large/006tNbRwly1fx0tm1hc9lj31kw0bzq70.jpg)








