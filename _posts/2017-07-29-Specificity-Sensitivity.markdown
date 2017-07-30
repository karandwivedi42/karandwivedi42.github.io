---
layout:     post
title:      Beyond CrossEntropy Loss
author:     Karan Dwivedi
tags:       idea
subtitle:   Balancing Specificity and Sensitivity in Medical Image Classification
category:   qure
---

Training a deep CNN model for binary classification with highly unbalanced classes presents many challenges. In medical imaging, it is particularly relevant as the input images (X-Ray Scans/ Head CT Scans) with an abnormality are much less than normal ones. If trained naively, the CNN (like resnet) collapses (or shows strong bias) to predict the majority class.

# Baseline Approaches

_(Suppose positive class has $n\_{1}$ samples and negative class has $n\_{2}$ samples where $n\_{2}/n\_{1} >= 7$)_

1. **Oversampling and Undersampling**: Usually all images in the dataset are sampled with equal probability per epoch. However, we can bias the sampling such that images from positive class have $k$ times more probability of being input, where $k \approx n\_{2}/n\_{1}$. The randomness in preprocessing (RandomCrop, Jitter etc.) helps prevent overfitting to some extent. The problem remains that when ratio is too high, random preprocessing is not able to prevent overfitting and the predictions initially look balanced but as training progresses, start getting biased towards majority class. Also, undersampling majority class effectively reduces the dataset size, and the model is not able to learn well.

2. **Weighted Loss**: [`nn.CrossEntropyLoss`](http://pytorch.org/docs/master/nn.html#torch.nn.CrossEntropyLoss) allows us to define a weight for each class. By keeping the class weight around $n\_{2}/n\_{1}$ for +ve class and 1 for negative class, we are able to get good performance. This approach however sufferes from same limitations as 1. By keeping a large enough weight, the model goes from being biased towards +ve class to reaching a stable point to getting biased towards negative class.

Because of the issues mentioned, we are almost never able to get most useful performance at end of training and have to resort to ad-hoc methods like

- experimenting with class weight
- choosing an intermediate validation epoch as final
- experimenting with oversampling and undersampling ratios etc.

# Review of Confusion Matrix and some terms:

| **Actual \ Predicted** | **0** | **1**
| **0** | A (True Negatives) | B (False Positives)
| **1** | C (False Negatives) | D (True Positives)

### Terms:

- **Accuracy**: $\dfrac{A+D}{A+B+C+D}$

- **Precision**: $\dfrac{D}{D + B}$

- **Recall/ Sensitivity/ tpr/ hit rate/ 1 - miss rate**: $\dfrac{D}{D+C}$

- **Specificity/ Inverse Recall/ 1 - fpr/ 1 - fallout**: $\dfrac{A}{A+B}$

- **F1 Score**: $\dfrac{2D}{2D + B + C}$

- **ROC Curve**: tpr v/s fpr (Recall v/s 1 - Inverse Recall) for varying thresholds

# Aim:

Usually medical practitioners are familiar with only a few of these metrics, like specificity and sensitivity. We want to maximise specificity and sensitivity together. With vanilla training, sensitivity is very low and specificity is close to 1 (Threshold 0.5 as usual). Also doctors do want AUC-ROC curves too.

While experimenting we will look at all these metrics ( also AUC-ROC and AUC-PR) and based on common sense choose which ones are the most reliable indicators of what we want.

# New approaches:

1. **Adaptive Tag Weight**

2. **Loss Functions**

3. **One shot Learning Approaches**


# Loss Functions

1. **Scalable Learning of Non-Decomposable Objectives**: Uses definition of AUCPR, AUCROC etc. to formulate new loss functions, which come out to be learnt weightings for positive and negative samples.

2. **Cost Sensetive CNN**: Explicitely learns a factor multiplied to misclassification (p,q) i.e. instance of class p misclassified as q. Read more [here]({% post_url 2017-07-30-CoSenCNN %})