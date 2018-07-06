---
layout: post
title:  "A Simple But Tough-To-Beat Baseline For Sentence Embedding"
date:   2017-12-05
categories: research
tags: paper-reading
---
You can find the paper at [ICLR'17](https://openreview.net/pdf?id=SyK00v5xx).

**TL;DR:** Word2Vec(CBOW) actually is a weighted average model, using weighted word average as sentence embedding can achieve a good result.

This paper is based on a latent variable generative process which is 

$$\mathsf{Pr}[w\ \textsf{emitted at time}\ t | c_t] \propto \exp(< c_t, v_w>),$$

in which view the probability of word $w$ appears at time $t$ in a sentence given discourse/context vector $c_t$ is proportional to $e^{c_t \cdot v_w}$.

The authors claim that the discourse vector $c_t$ is based on some *slow random walk process*, they further rewrite the latent variable generative model to

$$\mathsf{Pr}[w\ \textsf{emitted at time}\ t | c_t] = \alpha p(w) + (1-\alpha)\frac{\exp(<\widetilde{c}_s,v_w>)}{Z_{\widetilde{c}_s}},$$

in which $\widetilde{c}_s=\beta c_0 + (1-\beta)c_s, c_0 \perp c_s$, and $Z_{\widetilde{c}_s} = \Sigma_{w\in V}\exp(<\widetilde{c}_s, v_w>)$. This shows that a word $w$ may have a non-zero probability if 

1. it is related to sentence discourse $c_s$, or
2. term $\alpha p(w) > 0$, which means it is a frequent word, or
3. $v_w$ is correlated to $c_0$.

After a few other equations, the final objective function looks like

$$arg\ \textsf{max}\Sigma_{w\in s}f_w(\widetilde{c}_s)\propto \Sigma_{w\in s}\frac{a}{p(w) + a}v_w,$$

where $a = \frac{1-a}{\alpha Z}$.

The algorithm is much simpler than the equations:

![Algorithm of Sentence Embedding]({{base_url}}assets/paper_club_004_algo.png)
