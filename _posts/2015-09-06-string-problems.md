---
layout: post
title:  "String Problems"
date:   2015-09-06
categories: coding
tags: leetcode lcs longest-common-substring longest-common-subsequence
---
# String Matching, or needle in haystack (KMP Algorithm)

> Given a string `target`, find the index of its first occurrence in string `source`. `-1` if such index does not exist.

There is a good [youtube video](https://www.youtube.com/watch?v=1k2KDhcO_uo) that describes KMP table generation pretty well and I hope you could check it out.

Another good resource is the [Wikipedia page of KMP](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm), which explains the purpose of KMP with great examples. But it becomes less straightforward when comes to the pseudo code part.

Okay, here comes the code, we divide the code into two parts, the first one is generating KMP table, which determines how should we move when there is a mismatch, and the second part is the matching function that does the matching according to our KMP table.

{% gist 7238f288bb28ebc3a088 %}

Several notes:

1. The `failure_table` (or `table` in this case) is for the target string (needle), not the source string.
2. Using `pos` and `offset` can be more convience than traditional `i`, `j` which denotes the position in each string accordingly. Because it eliminates the calulation of the index of first occurrence.

The above code is also the solution of `strStr()` question on [LeetCode](https://leetcode.com/problems/implement-strstr/description/) and LintCode.

# Longest Common Substring

> Find the longest common substring of two strings.

## Suffix Tree solution of LCS with two strings

First let’s see an examle from Wikipedia

```
ABABC
  |||
 BABCA
```

Apparently the longset common substring is `ABC`, and `ABC` can be obtained by the following procedures:

1. For all suffixes of `ABABC` and `BABCA`, find the longest common prefix of their suffixes
2. For all prefixes of `ABABC` and `BABCA`, find the longest common suffix of their prefixes

The defintion and implementation of Suffix Tree or Suffix Trie can be found in [another article](trie-prefix-suffix-tree).

Here is the naive solution of it, and the time complexity is `O(n^2 + m^2)`, where `n` and `m` are the length of each string.

The main steps of this solution is:

1. we insert all suffixes of string A and string B into a suffix trie. <- this costs `O(n^2 + m^2)` due to the naive implementation
2. we search the suffix trie we get and find the deepest ancestor of suffixes of both A and B. <- this costs `O(m+n)` time

{% gist b06602a60aa5feb47497 %}

An improved implementation is as follow. 

> TODO: gist of an improved version of suffix tree

Here is a youtube video that explains how [Ukkonen’s algorithm](https://www.youtube.com/watch?v=aPRqocoBsFQ) works.

## Dynamic Programming solution of LCS with two strings

{% gist 857586e520357160bd0b %}

## Find the longset common substring of m strings.

Suffix Tree solution of LCS with `m` strings.

# Longest Common Subsequence

