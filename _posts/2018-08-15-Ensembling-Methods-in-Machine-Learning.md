---
layout:     post
title:      Ensembling Methods in Machine Learning
subtitle:   Bagging and Boosting
date:       2018-08-15
author:     Ed He
header-img: img/bb2.png
catalog: true
tags:
    - Machine Learning
---


# Introduction

Ensemble methods are meta-algorithms that combine several machine learning techniques into one predictive model in order to decrease variance(*bagging*), bias(*boosting*), or improve predictions(*stacking*).

Ensemble methods can be divided into two groups:
* sequential ensemble methods where the base learners are generated sequentially (e.g. AdaBoost).
The basic motivation of sequential methods is to exploit the dependence between the base learners. The overall performance can be boosted by weighing previously mislabeled examples with higher weight.
* parallel ensemble methods where the base learners are generated in parallel (e.g. Random Forest). 
The basic motivation of parallel methods is to exploit independence between the base learners since the error can be reduced dramatically by averaging.

Most ensemble methods use a single base learning algorithm to produce homogeneous base learners, i.e. learners of the same type, leading to homogeneous ensembles.There are also some methods that use heterogeneous learners, i.e. learners of different types, leading to heterogeneous ensembles. In order for ensemble methods to be more accurate than any of its individual members, the base learners have to be as accurate as possible and as diverse as possible.

# Bagging
Stands for bootstrap aggregation, e.g. Random forests.

# Boosting
Refers to a family of algorithms that are able to convert weak learners to strong learners. The main principle of boosting is to fit a sequence of weak learners− models that are only slightly better than random guessing, such as small decision trees− to weighted versions of the data. More weight is given to examples that were misclassified by earlier rounds. e.g. Adaboost. The principal difference between boosting and the committee methods, such as bagging, is that base learners are trained in sequence on a weighted version of the data.

# Stacking
An ensemble learning technique that combines multiple classification or regression models via a meta-classifier or a meta-regressor. The base level models are trained based on a complete training set, then the meta-model is trained on the outputs of the base level model as features.
