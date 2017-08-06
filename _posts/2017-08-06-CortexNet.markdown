---
layout:     post
title:      Predictive Coding
author:     Karan Dwivedi
tags:       paper-summary
subtitle:   Review of PredNet and CortexNet papers
category:   iitd
---

This post is about the PredNet and CortexNet papers. I worked on them briefly while beginning to learn pytorch and am now trying to use them in my Mini-P (Video frame interpolation)

# Also read:

1. Prof. Culurciello talks about Prednet on [his medium blog](https://medium.com/towards-data-science/a-new-kind-of-deep-neural-networks-749bcde19108)

# [CortexNet](https://engineering.purdue.edu/elab/CortexNet)

### Motivation

We learn a lot in unsupervised setting. Babies learn to track faces. We learn to predict and visualise. Our visual system is robust to perturbations. Inspired by human visual cortex, the failry generic architecture models top-down (feedforward), bottom-up (feedback) and lateral (temporal) connections.

__Interesting Claims: The model learns to (1) compensate for camera egomotion, (2) predict object trajectory and (3) give attention to one object at a time.__ (However I don't find explicit experiments demonstrating the same.. at least point 3 should be.)

### Model architecture and losses

[TODO] However, basically see Figure 1 and read its description first. Then read Section 3. Then see figure 2. Then read its description. Then read the text below it.

### Experiments

1. Predicting future frames

2. Video Classification

### Results

I don't see proper comparision and results etc. But a friend has said that this worked for him above all other approaches in a related project on video stylization.

### Code

**Amazing code is available at their [github repo](https://github.com/atcold/pytorch-CortexNet)**

Definitely go through it if you are learning pytorch.

# [PredNet](https://coxlab.github.io/prednet/)

### Relation to CortexNet

1. Ladder sort of network

2. Predictive Coding

3. Predecessor to CortexNet

### Differences:

1. Uses ConvLSTM

2. Much better experiments and results.