---
layout: post
title:  "StarSpace, embed anything!"
date:   2017-09-21
categories: research
tags: paper-reading
---

This is a new paper submitted to AAAI'18 by Facebook AI Research (FAIR) and the full article can be found on [Arxiv](https://arxiv.org/abs/1709.03856), the code is available at [GitHub](https://github.com/facebookresearch/Starspace). One of the author [Antoine Bordes](https://research.fb.com/people/bordes-antoine/) is also the author of TransE, a well-known Knowledge Graph Completion model.

In general, StarSpace is capable of learning embeddings from a set of finite features. If we have a finite feature dictionary $\mathcal{D}$ and has a feature matrix $F \in \mathbb{R}^{\mathcal{D}\times d}$, in which $F_i$ is the $d$-dimensional embedding of $i^{\mathsf{th}}$ feature. Then for an entity $a$ with k features $\mathbf{a}=\{i_1,\cdots,i_k\}$, the embedding of entity $a$ is defined as unweighted sum of all these feature embeddings $\sum_{i\in\mathbf{a}}F_i$.

When training the model, StarSpace uses 

$$\sum_{(a,b)\in E^+, b^- \in E^-} L^{\textsf{batch}}(sim(a,b),sim(a,b^-_1),\ldots,sim(a,b^-_k))$$

in which

* $E^+$ is the positive entity pair set.
* $E^-$ is the negative pair set that $\{(a,b) | (a,\cdot) \in E^+, (a,b) \notin E^+ \}$, which means $a$ and $b$ has the correct left-hand and right-hand entity type but $b$ is not the correct right-hand element. For example if we are predicting LocatedIn, then $a$ will be a building and $b$ will be a location.
* The sim function, according to the authors, is best to be inner product for smaller number of features ($|\mathbf{a}|$) and cosine for larger number of features.
* $\mathcal{L}$ is better to be ranking loss instead of softmax loss. (What is the ranking loss in this case? It should not be the standard pairwise ranking loss I guess)

It is relatively easy to do the multi-class classification because the labels can be viewed as $b$, same thing applies to word/sentence/document similarity in which each pair of similar word/sentence/document can be represented by $a$ and $b$. As for Knowledge Graph Completion, $a$ could be h & r and $b$ be t or $a$ be h and $b$ be r & t.