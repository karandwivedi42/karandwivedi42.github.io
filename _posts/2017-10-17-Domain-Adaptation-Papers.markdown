---
layout:     post
title:      Unsupervised Domain Adaptation
author:     Karan Dwivedi
tags:       paper-summary
category:   qure
subtitle:   Unsupervised domain adaptation can be easily extended to semi supervised / supervised approaches.
---

On a broad level, there are 2 methods for domain adaptation in Deep CNN type networks:

1. *Correlation Alignment:* Aligning the global statistics of source and target domain (handcrafted transformation). 

2. *Adversarial Alignment:* Learns to form domain-invariant representations (learnt transformation).

## Adversarial Alignment

### Reading:

![Description](/img/DomainAda/adversarial.png)

1. The source and target models can be same or different. To make them same, we use the weight tying concept in CNNs. Or we can tie only some layers.

2. The classification loss is only on source domain. We can also add it on target domain if we have data (semi-supervised DA).

3. The domain confusion loss can be of different types:

    1. Adversarial loss: Add a classifier that learns to distinguish between the 2 domains. However, reverse the gradient going into the main network. Problem: The loss is not good enough. Suppose it learns to distinguish perfectly, and then reverses its decision. That will perform well on this loss but not help us.

    2. GAN Loss: Intuitively, this is like matching a varying fake distribution to a constant real distribution. It only penalizes classification of target class, leaving the source feature distribution as it is (unless tied weights I guess).

    3. Confusion Objective: Adversarial Loss + Reverse Adversarial Loss.

### Ideas:

1. Keep a pretrained CNN and a new CNN initialised with pretrained CNN, with top few layers unfrozen. Learn a GAN loss and only propogate it in the new model. We can replace GAN Loss with pairwise L2 loss also.

2. DANN: Pretrained CNN + All weights tied. Learn domain invariance but keep a classification loss to ensure representations stay meaningful.

### Papers:

1. [Adversarial Discriminative Domain Adaptation](https://arxiv.org/abs/1702.05464)

2. [Domain-Adversarial Training of Neural Networks](http://jmlr.org/papers/volume17/15-239/15-239.pdf)

## BatchNorm based Alignment

### Reading:

1. Both shallow layers and deep layers of the DNN are influenced by domain shift. Domain adaptation by manipulating the output layer alone is not enough.

2. The statistics of BN layer contain the traits of the data domain.

Method:

1. Compute mean and var of target domain. While testing use target domain mean and variance instead of source domain.

### Ideas:

1. Visualize mean and variance for each mini-batch for each layer for both datasets.

2. If they come to be very different for the 2 datasets, then the hypothesis in the paper is true.

## Unified Deep Supervised Domain Adaptation and Generalization

This is for *supervised* domain alignment with scarse target data.

### Reading:

Conceptually, we try to bring source and target domain features closer for points in same class, and try to separate the points in source and target domains for different classes.

![Description](/img/DomainAda/ccsa.png)

The $d$ loss is for points in same class. The $k$ loss is for points in different classes.

### Ideas:

1. Label some points in target domain. Now fine-tune the last layer of CNN with 4 objectives: 2 Classification losses and 2 feature distance losses.

2. Keep 2 networks - fixed pretrained and fine tune last layer of the other (init with pretrained). Train with cls loss on 2nd + 2 feature distance losses between 1st and second.


## [CORAL and DEEP CORAL](https://arxiv.org/abs/1607.01719)

The idea is to align second order statistics (Covariance) of source and target domains. I don't think this will work with scarce target domain as there are too little target images compared to source.

## [Joint Geometrical and Statistical Alignment for Visual Domain Adaptation](https://arxiv.org/abs/1705.05498)

They haven't compared their approach to any adversarial approach.