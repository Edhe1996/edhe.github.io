---
layout:     post
title:      mAP in Person Re-ID
subtitle:   How to calculate mAP
date:       2018-08-20
author:     Ed He
catalog: true
tags:
    - Machine Learning
    - Person re-ID
---


## Introduction
Mean average precision (mAP) has been widely used in object detection and image retrieval tasks, especially the person re-identification task. Here I will illustrate the calculation of mAP for person re-ID.

## Calculation
![mAP](https://github.com/Edhe1996/edhe1996.github.io/blob/master/img/mAP.jpg)
![area](https://github.com/Edhe1996/edhe1996.github.io/blob/master/img/area.jpg)
Briefly, average precision (AP) is the area under the precision-recall curve. In person re-ID, the gap between every recall value can be calculated as `recall - old_recall = 1 / num_of_true_matches`. The the area is easy to get.

```python
for i in range(num_of_good):
    # The gap between every recall value
    d_recall = 1.0 / num_of_good
    precision = (i + 1)*1.0 / (rows_good[i] + 1)
    if rows_good[i] != 0:
        # The last precision, so i = i + 1 - 1
        old_precision = i * 1.0 / rows_good[i]
    else:
        # Avoid zero
        old_precision = 1.0
    # ap is the area under the p-r curve
    ap = ap + d_recall * (old_precision + precision) / 2
```
And the mAP is just the average over all query images.
