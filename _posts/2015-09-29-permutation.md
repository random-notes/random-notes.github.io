---
layout: post
title:  "Permutation"
date:   2015-09-29
categories: coding
tags: leetcode permutation
---
# Next Permutation (With duplication)

First let’s check the first six permutation of sequence `[1,2,3,4]` in lexical order.

```
1 2 3 4
1 2 4 3
1 3 2 4
1 3 4 2
1 4 2 3
1 4 3 2
```

If we starting from the tail of each permutation and try find an ascending sequence `[n-1 : j]`, then the discovered sequences are:

```
1 2 3 [4]
1 2 [4 3]
1 3 2 [4]
1 3 [4 2]
1 4 2 [3]
1 [4 3 2]
```

Now for every permutation we successfully divide it into two parts, then if we exam the difference between two adjacent permutation, we can find the following rules:


1. get s[j - 1]
2. find the first s[k] in s[n-1:j] that is larger than s[j-1]
3. swap s[j-1] and s[k], sort s[j:n-1] in ascending order


> Find ascending subarray starting from the end, the left element of this subarray should be replaced by the first element at the right of the subarray that is larger than the element.

{% gist 4f13804d7e34c9d955ad %}

# Previous Permutation (With duplication)

> Find descending subarray starting from the end, the left element of this subarray should be replaced by the first element at the right of the subarray that is smaller than the element.

{% gist ec0f330b300e77c3529c %}

# Permutation (Without duplication)

We can do a depth-first search to construct the permutation, or use the code snippet we shown above to generate next permutation until reach `asc == 0`. Note the result of both solution has the same order.

The DFS method is maintaining a visited vector and for every recursive call pick one unvisited node and visit it until all nodes are visited (put into the temporary result vector).

{% gist b6fe650dfe1af6209c86 %}

# Permutation (With duplication)

When there are duplications in `nums`, create a map in which the key is the number in `nums` and the value is the count of the occurrence of key. When doing DFS, traverse through the map and try any numbers that has a value larger than `1`.

{% gist d9ea51e11fcc9c77e21e %}