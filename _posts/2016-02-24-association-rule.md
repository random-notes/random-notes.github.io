---
layout: post
title:  "Association Rule Basics"
date:   2016-02-24
categories: research
tags: association-rule
---

> $X \Rightarrow Y$ means X associated with Y.

# Support

The portion of transactions that contain item set X.

This number presents how specific item set X is.
# Confidence

The portion of transactions that contain both item set X and Y.

This number presents/estimate the conditional probability that Y exits when X exists.
# Lift
$lift(X \Rightarrow Y) = \frac{supp(X\cup Y)}{supp(X) \times supp(Y)}$

The ratio of the observed support to X and the assumption that X and Y are independent.
# Conviction
$conv(X \Rightarrow Y) = \frac{1-supp(Y)}{1 - conf(X \Rightarrow Y)}$

The ratio of the expected frequency that X occurs without Y.