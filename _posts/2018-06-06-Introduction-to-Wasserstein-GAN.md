---
layout:     post
title:      Introduction to Wasserstein GAN
subtitle:   An advanced version of GAN
date:       2018-06-06
author:     Ed He
catalog: true
tags:
    - Machine Learning
    - GAN
    - Deep Learning
---


## Introduction
Generative Adversarial Network (GAN) is a very popular generative model in recent years. In this blog, I will give a brief introduction to one of its version: Wasserstein GAN.

## Generative model
Typically, to form a generative model, we need to learn a probability distribution, and the classic solution is to learn a probability density. Usually a parametric family of densities (P_\theta)_{\theta \in R^d} is defined and the one that can maximise the likelihood of the data the density we need. If the real data distribution $P_r$ has a probability density and P_\theta is the probability distribution of the parametrised probability density $P_\theta$, this is equivalent to minimise the Kullback-Leibler divergence $KL(P_r||P_\theta)$.

Under some circumstances, the density $P_\theta$ may do not exist. To address this issue, a Gaussian noise with relatively high variance is added to cover all the examples. However, the added noise will degrade the quality of the samples, which means it is incorrect for the problem, but is necessary to conduct a maximum likelihood on the model.

An alternative way is to define a random variable $Z$ with a fixed probability distribution $p(z)$ and map it through a parametrised function $g_\theta:Z \rightarrow X$ that can directly generate data which follow a certain distribution $\mathbb{P}_\theta$ without estimating the density of real distribution $P_r$. Usually the function is a neural network. For the consideration of image superresolution or data augmentation, the ability to generate synthetic samples is often more important than finding the numerical value of the probability density.

GAN and Variational Auto-Encoder (VAE) are both popular examples of this method. VAE mainly draws attention to the approximate likelihood of the samples, thus share the limitation of the first method mentioned before. GAN is more flexible with Jensen-Shannon distance as the measurement of two different distributions. Intuitively, GAN is implemented by a system of two networks (generator and discriminator) contesting with each other. The generator network takes random numbers as input and is trained to output samples that can cheat the discriminator, and the discriminator is optimised to tell a sample is synthetic or real. However, the training of GAN is unstable due to some reasons that are well investigated in the paper. Wasserstein GAN was developed to deal with the many problems remained in the training of GAN by directing attention on the various methods to measure the distance between the real distribution and model distribution.
