---
layout: post
title:  "Learning Edge Representations via Low-Rank Asymmetric Projections"
date:   2017-09-24
categories: research
tags: paper-reading
---
This paper is published at CIKM'17 and can be found at [Arxiv](https://arxiv.org/pdf/1705.05615.pdf) and the code is available at [here](http://sami.haija.org/graph/deep_embedding.html) however as of Sept 24th it is showing `Coming Soon!`. The corresponding author of this work is the author of [DeepWalk](https://arxiv.org/abs/1403.6652).

Most network representation models learn representations $Y_u$ for each node $u$ in the graph and measure the closeness/edge betwee two nodes $u$ and $v$ by some distance measurement $\mathsf{dist}(Y_u, Y_v)$. In this paper they call this method as **node-centric** and has two drawbacks:

1. Such method implicitly assumes the connections are undirected, which means $u\rightarrow v$ and $v\rightarrow u$ are equivalent because they are sharing the same distance function,
2. The authors claim that in some cases, the representation matrix $\shortmid V \shortmid \times d$ would be larger than the sparse adjacent matrix which has $\shortmid E \shortmid $ elements.

In this work the authors propose a method to learn the node embeddings as follow:

For each node $u$ in the graph, they learn an embedding $Y_u \in \mathbb{R}^D$. Then they pass it through a shared deep neural network denoted by $f:\mathbb{R}^D\rightarrow \mathbb{R}^d$ to reduce the dimension from $D$ to $d$. They also create an edge likelihood function $g(u,v):f(Y_u)^T\times M \times f(Y_v)$ to represent the edges. The matrix $M$ is a low-rank matrix which is defined as $M = L \times R$, $L = \mathbb{R}^{d\times b}$, $R = \mathbb{R}^{b\times d}$. The edge representation is a combination of two vectors, the source embedding $Rf(Y_v)$ and the destination embedding $L^Tf(Y_u)$.

![Model Illustration]({{site.url}}/assets/paper_club_002_edge_rep.png)

*Although one could use the trained $d$-dimensional embeddings for inference, this model still needs to learn two $\shortmid V\shortmid \times D$ embedding matrices during training like other models with an extra DNN.*

The DNN in this work is a two layer fully-connected layers with batch normalization and uses relu as the activation function.

Like word2vec, in this work the model also learns two representations $Y_u^{\textsf{source}}$ and $Y_u^{\textsf{dest}}$ for a single node $u$. Then the edge likelihood of $(u,v)$ and $(v,u)$ would be $\textsf{dist}(Y_u^{\textsf{source}},Y_v^{\textsf{dest}})$ and $\textsf{dist}(Y_v^{\textsf{source}},Y_u^{\textsf{dest}})$ respectively.

The difference between this work and their previous DeepWalk are threefold:

### The Graph Likelihood Function

Instead of minimizing the usual loss function used in other random walk based network representation models which is usually defined as

$$\underset{\theta}{\mathrm{argmin}}\prod_{v\in V}\prod_{c \in N(v)} p(c\shortmid v;\theta)$$

in which $N(v)$ is the neighborhood node set of node $u$ defined by a random walk path and a contex window size (you could find the details in DeepWalk).

This work defines a new loss function based on the idea of logistic regression

$$\Pr(G) \propto \prod_{u\in V, v\in V} \sigma(g(u,v))^{\mathcal{D}_{u,v}}(1-\sigma(g(u,v)))^{\mathbb{I}((u,v)\notin E_{train}}$$

in which the first term 

$$\prod_{u \in V, v \in V}\sigma (g(u,v))^{\mathcal{D}_{u,v}}$$ 

is enabled when $u$ and $v$ co-occur in the same context window by the unnormalized requency 

$$\mathcal{D}_{u,v}$$ 

and 

$$(1-\sigma(g(u,v)))^{\mathbb{I}((u,v) \notin E_{train}}$$ 

is enabled when $u\rightarrow v$ is a false edge. Note that the authors did not specify if $(u,v)\notin E_{train}$ only considers direct connection or multi-hop connection.

Either way, their final loss function is more straightforward 

$$\mathcal{L} = \mathbb{E}_{(u,v) \sim \mathcal{D}/Z}\left[\log\sigma(g(u,v)) + \sum_{v^-\in Sample(K, u\hat{-})}\log(1-\sigma(g(u,v^-)))\right]$$

By explicitly selecting true node pairs and false node pairs, one no longer needs to worry about the aforementioned problem in the $\Pr(G)$ equation.

### The Edge Likelihood Function

This is the $g(\cdot)$ we discussed previously.

### The Context Definition

The context is defined differently for undirected graphs and directed graphs. For paths in undirected graphs, context of $u_i$ in $u_1\rightarrow\cdots\rightarrow u_i\rightarrow\cdots\rightarrow u_k$ are $\{u_1,\cdots,u_{i-1},\cdots,u_{k}\}$; whereas in directed graphs, the context of $u_i$ becomes $\{u_{i+1},\cdots,u_k\}$. But I'm not sure why this would improve the performance because the previous nodes should also play a role when defining the embedding.
