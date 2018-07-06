---
layout: post
title:  "Pirority Queue, Heap Implementation"
date:   2015-09-11
categories: coding
tags: leetcode heap pirority-queue
---

Pirority queue can be implemented by a binary heap, and heap is usually implemented in a vector so we do not need to keep multiple pointers that point to its parent and two children.

If we use a vector `v` to store a heap, it satisfies the following rules:


1. the root of this heap is stored at `v[1]`, and `v[0]` is never used
2. for any sub heap in this vector, if `v[pos]` is the root, `v[2*pos]` and `v[2*pos + 1]` are the children of it.
3. for any node (except the root) in the heap, `v[pos]`‘s parent is `v[pos/2]`.

The main operations of a heap is `insert` ant `pop`, when the heap is a min heap, `pop` pops out the maximum value in the heap and when it is a max heap, it pops out the smallest one. Since we need to keep the root to be the smallest or largest element, every time we modify the heap, we need to reorder it in order to keep the root is the largest or smallest.

Therefore, we need to define two algorithms to reorder the heap for both `insert` and `pop` function.

## Insert

Insert is done by attach an element to the rightmost node.