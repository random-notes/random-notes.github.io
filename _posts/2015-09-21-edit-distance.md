---
layout: post
title:  "Edit Distance"
date:   2015-09-21
categories: coding
tags: leetcode edit-distance levenshtein-distance
---
The sub-problem of edit distance is the edit distance of string `A[0:i]` and string `B[0:j]`.

The boundary cases are:

1. `A.length() == 0`, then edit distance is `B.legnth()`
2. `B.length() == 0`, then edit distance is `A.length()`
3. otherwise, there are three cases (log the minimum score):
	1. if we align `A` and `B` at `i` and `j` respectly, then the edit distance is `M[i-1][j-1] + (A[i] == B[j] ? 0 : 1)`
	2. if we align by `A[0:i-1]` and `B[0:j]`, then we need to perform a delete/add operation
	3. if we align by A`[0:i]` and `B[0:j-1]`, then we need to perform a delete/add operation

By analysis the above conditions, we can know the state we need to keep is two rows, namely `M[i-1][0:j]` and `M[i][0:j]`.

Therefore, the code is:

{% gist 264b131e2edb52d2ba5b %}