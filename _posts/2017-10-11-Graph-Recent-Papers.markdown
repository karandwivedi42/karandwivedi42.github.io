---
layout:     post
title:      Graph Neural Networks
author:     Karan Dwivedi
tags:       paper-summary
category:   arbit
subtitle:   Recent Vision application papers of Graph Neural Networks
---


## [Situation Recognition with Graph Neural Networks](https://arxiv.org/pdf/1708.04320.pdf)

### What is Graph Neural Network?

A GNN defines observation and output of each node and propogates messages along the edges in recurrent manner.

### Terminology

- Verbs: Repairing, Driving

- Nouns: Hammer, Car

- Roles: Agent, Vehicle, Place

- Frames: Set of roles and associated nouns.

### Task

Given image, predict verb and noun for each role.

### Motivation to use Graph

Capturing dependencies between roles and verbs. Eg. `Verb: Repairing` strongly suggests `Agent: Screwdriver` and `Place:Workshop`. Similarly role `Item: something heavy` implies something about other role like `Agent`.

### Graph Formulation

- Nodes are: verb (1 per graph), role.

- Edges are: dependancy between verb-role or role-role pair (right now undirected, so I think more like joint).

- Each node instantiated with repr.s from pretrained verb/ noun (given role) detection CNN.

### Comments

- CNN + CRF is a very weak baseline. Capturing dependencies in important.

- What about bilinear pooling/layers? They also capture feature interactions.

- Very specific supervision (unlike VQA or Image Captioning).

## [3D Graph Neural Networks for RGBD Semantic Segmentation](http://www.cs.utoronto.ca/~fidler/papers/3dggnn_iccv17.pdf)

- Image -> 2D CNN -> pointwise output.

- Depth Map -> Point Cloud -> Connect NN -> Graph -> Initialize node with CNN output (1 pixel = 1 node).

- Trained end to end (CNN + GGNN).

- For graph neural network, they use same as above.

## [The More You Know: Using Knowledge Graphs for Image Classification](https://arxiv.org/pdf/1612.04844.pdf)

- VGG-fc7 features of image + sorted stach graph node outputs -> Multilabel Classifier (Task: Multilabel cls)

- Graph over: Object-object and object-attribute relationship

- Dataset: Visual Genome (modified)

## [Graph-Structured Representations for Visual Question Answering](https://arxiv.org/pdf/1609.05600.pdf)

- Dataset has scene description -> Scene Graph

- Question -> Parsing tools -> Question Graph

## [Image Retrieval using Scene Graphs](http://cs.stanford.edu/people/jcjohns/papers/cvpr2015/JohnsonCVPR2015.pdf)

```
This paper develops a novel framework for semantic image
retrieval based on the notion of a scene graph. Our
scene graphs represent objects (“man”, “boat”), attributes
of objects (“boat is white”) and relationships between objects
(“man standing on boat”). We use these scene graphs
as queries to retrieve semantically related images. To this
end, we design a conditional random field model that reasons
about possible groundings of scene graphs to test images.
The likelihoods of these groundings are used as
ranking scores for retrieval.
```


## [Pixels to Graphs by Associative Embedding](https://arxiv.org/pdf/1706.07365v1.pdf)

- Reread: Fundamental doubt: What do these features learn? Do they learn everything important and nets on top of it are just readouts? When does it succeed and when does it fail? Hoe does it induce sparsity (if hardcoded threshold then not good)?

- Associative Embeddings

- Image -> Features (Stacked Hourglass)

- Features -> 2 Heatmaps (Object, Relationship) < BCE Loss

- Features -> Classification, BBox (Separate network) < Usual I guess

- Features -> Embedding (1 for vertex, 2 for edge) < Pull apart same, push away different < Sim Losses

- Step 1: Predict and Step 2: Join

- On high level, it seems a lot of things can be improved, like:
    
    - Heatmap idea: penalises even small shift in detection.
    - Edges: Grounding edges by their centre seems unintuitive (but unavoidable?).
    - Classification of each object happens in isolated networks. We lose some semantic relationship info here.