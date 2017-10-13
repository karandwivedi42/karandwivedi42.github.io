---
layout:     post
title:      Graph Neural Networks (Defintions)
author:     Karan Dwivedi
tags:       paper-summary
category:   arbit
subtitle:   Different definitions of Graph Neural Networks
---
Edit: The paper "Neural Message Passing for Quantum Chemistry" is good for a TL;DR of this field.

The terms "Graph neural Networks" and "Graph Convolution Networks" are very generic. Let's see what the different mathematical definitions are:

1. [The Graph Neural Network Model (Scarselli et al., 2009)](http://ieeexplore.ieee.org/document/4700287/)

    - Information diffusion mechanism - information exchange among neighbors till equilibrium (`norm(x[t+1] - x[t]) < eps.`).

    - Sort of universal approximation proven and can approximate useful functions.

    - Dummy super node for graph level classification

    - Diffusion is constrained (Banach theorem)

    - Output function:  `x -> o` and transition function: `x[t] -> x[t+1]`.

    - Propogate messages among neighbors till equilibrium reached (exponentially fast). Then use output function. Then BPTT.

    - order of neighbors matters or not: Positional (sorted list and fill nulls) and non positional graphs (decompose fn. into addition of smaller function).

2. [Gated Graph Sequence Neural Networks](https://arxiv.org/abs/1511.05493v4)

    Based on above with improvements:

    - Fixed number of timesteps - Needs more memory but now parameters are unconstrained.

    - Use GRU.

    - Node Annotations: Initialization matters now (no equilibrium achieved).

    - Output models: Softmax over outputs, Soft attention over outputs.

    - GG *Sequence* -NN: 2 GG-NN - One for `X[k] -> o[k]` and other for `X[k] -> X[k+1]`.

    - Tasks: BaBi, Program veirifcation.

3. [Neural Message Passing for Quantum Chemistry](https://arxiv.org/abs/1704.01212)

    - First, they review current approaches in a unified framework (see appendix for how Kipf model fits in message passing framework).

    - Input: Set of feature vector for nodes and adjecency matrix (type of edge + distance between nodes connected).

    - Model: GRU; weight tying at each time step.

    - Message functions:

        1. Matmul: f(Neighbor node)*Adjacency value of edge

        2. Edge conditioned: NN for edge vector -> matrix.

        3. Pair message: NN for edge, source, dest -> message

    - Virtual Graph elements:

        1. 'Virtual' typed edges for all non neighbor nodes (long message passing).

        2. 'Latent master node' (scratch space).

    - Readout functions:

        1. Simple: GG-NN

        2. Compicated: Set2Set

    - Multiple towers - Split embeddings for lower complexity