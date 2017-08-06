---
layout:     post
title:      Importance Sampling
author:     Karan Dwivedi
tags:       paper-summary idea
subtitle:   Importance sampling focuses the computation to informative/important samples 
---

This post is about the idea of Importance Sampling. I will review the paper: [Biased Importance Sampling for Deep Neural Network Training](https://arxiv.org/abs/1706.00043)

### What is Importance Sampling?

When training a model, it is obvious that not all samples are equally important; many of them are properly handled after a few epochs of training, and most could be ignored at that point without impacting the resulting final model.

### Biased Importance Sampling for Deep Neural Network Training

In summary, the contributions of this work are:
- The use of the loss instead of the gradient norm to estimate the importance of a sample
- The creation of a model able to approximate the loss for a low computational overhead
- The development of an online algorithm that minimizes a soft max-loss in the training set through importance sampling.
- They also show how this method leads to better generalization.

#### Questions

1. What about catastrophic forgetting? Unimportant samples might become important later.

2. How to use this for self supervision in presence of large amount of data where we may not have access to complete training data at the same time?

3. Keeping a separate model and using loss on that seems somewhat redundant. Why not get some signal from the same model?

They have also open sourced their [code on github](https://github.com/idiap/importance-sampling) (related [reddit thread](https://www.reddit.com/r/MachineLearning/comments/6rde9t/r_keras_library_for_deep_neural_network_training/))
