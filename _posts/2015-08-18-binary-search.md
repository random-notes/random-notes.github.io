---
layout: post
title:  "Algorithm Problems: Binary Search"
date:   2015-08-18
categories: coding
tags: binary-search leetcode
---
# Binary Search Basics
If you know what is binary search, I encourage you to skip the first section and goes to [the questions](#questions).

Before we dive into real problems, I want to briefly discuss about binary search. Binary search is a technic that can search for the existance of a value in a sorted array in `O(logn)` time, where `n` is the size of that array.

An example array `v`, which is sorted, is shown below:

```
index: 0  1  2  3  4  5  6  7  8  9  10
value: 1  3  4  5  7  9 11 13 20 21  39
```

Say we want to determine if `10` is in the array or not, an intuitive solution is walk though this array and check `arr[i] == 9`. The time complexity of such algorithm is `O(n)`. Note that this is usually the best solution if the array is unsorted because binary search can not be applied to unsorted array (A rotated, sorted array is an exception and we will talk about it later).

To reduce the time complexity of searching for a value in sorted array from O(n) to O(logn), we will use binary search next.

The `C++` snippet is as follow:

{% gist 852d366d94387f4ffc16 %}

Let’s walk though the code using our example:

At the very beginning, `l = 0` and `r = 10`. `v[l:r]`, where `l:r` means `[l,l+1,...,r-1,r]`, denotes the search range, in other words, we know that if the target value exists in `v`, it must lies in `v[l:r]`.
```
index: 0  1  2  3  4  5  6  7  8  9  10
value: 1  3  4  5  7  9 11 13 20 21  39
       ^                              ^
label: l                              r
```
Then we compute the median `v[m]` of `v[l:r]`, which is the value right in the middle of `v[l:r]`. Note that we are not necessary computing the exact median when there are even number of elements in the range. That is to say, we are calculating $v[ \lfloor \frac{l+r}{2} \rfloor ]$.
```
index: 0  1  2  3  4  5  6  7  8  9  10
value: 1  3  4  5  7  9 11 13 20 21  39
       ^              ^               ^
label: l              m               r
```
Then we test `v[l]` and `v[r]` to see if they equal to `10`. Unfortunately none of them is `10` therefore we say `10` is not in this array `v`. One may wondering why comparing `v[r]` since `r` is `m` in the previous step and we already compared `v[m]` in last step. Thing may get tricky when one is trying to save this two comparisons. An example is what if `l == m`? In such case we do not need to compare `v[l]` but `v[r]`.

# Questions
There are five main parts of a binary search algorithm:

1. Invalid input detection. One need to filter our all input with a length less than `0` because this will make `r == -1`.
2. Iteration stop condition. Usually I like to use `l < r - 1` because it is easy to implement and can avoid specific cases. But you can always use `l < r - k`, where `k >= 0`.
3. Pruning condition. The classic binary search uses `v[m] < target` or `v[m] > target`, but this depends on your problems.
4. Boundry condition. By using `l < r - 1`, one do not need to do detecting but check the value of `v[l]` and `v[r]`.
5. Boundry check. Check boundary values `v[l]` and `v[r]` and deal with the case when target is not found.

## Problems on Sorted Arrays

### Binary Search
The question description can be found at Lintcode: (14) Binary Search. The solution is described in [Binary Search Basics](#binary-search-basics).

### Search Insert Position
The question description can be found at Lintcode: (60) Search insert position and Leetcode: (35) Search insert position.

The answer of this question is a twisted version of classic binary search. Namely we just need to change the invalid input detection and boundry check.

The invalid input detection should output `0` instead of `-1` because when target is not found, the output should be the index of target if it was inserted. In this case, it is `0`.

As for boundary check, we return `l` or `r` if `m[l] == target` or `m[r] == target`. If none of them equals to `target`, we need to determine where to insert `target`. The index should be the smallest value in `m` that is larger than `target`.

{% gist 2d48d066fee6ccc38c85 %}

### Wood Cut

### Find Bad Version

### Find the nearest pow(i,2) that equals or less thansqrt(x)
The question description can be found at Lintcode: (141) sqrt(x) and Leetcode: (69) sqrt(x).

This problem is also a variant of binary search problem. One thing we need to mention is how to calculate `sqrt(2)`. The question does not require you to return `1.414` but `1` (the largest integer that satifies `x^2 < target`). Therefore, we need to change the pruning condition to comparing `m[mid]^2` and `x`.

{% gist 15f9e8aed2f0590ba099 %}

### Find value range in sorted array containing duplicate values

The question description can be found at Lintcode: (61) Search for a Range and Leetcode: (34) Search for a Range.

If we keep using the [vanilla binary search](#binary-search-basics), we can find `v[index] = target`, but we can not guarantee which index we would return. An example is:

```
index: 0  1  2  3  4  5  6  7  8  9  10
value: 1  3  3  3  4  9 11 13 20 21  39
             ^
label:       m
```

Here we return `index = 2`, but we can not return the correct range of `3`, which is `[1,3]`.

A simple solution is search the left and right part of `index` one by one until we find an element not equals to `target` or reaches the boundary. This is doable but in the worse case it will cost `O(n)` time, which is not what we want.

To solve this problem in `O(logn)` time as usual, we need to twist the traditional binary search algorithm. Remember, we keep reducing the search space by comparing `v[mid]` and `target`, and we stop when `v[mid] == target`. What if we change the comparison? I do not know how to search the range in one binary search, so I break the result into two parts, namely find the left most `target` and the right most `target`.

To find the left most `target` in this array, we should change the comparison from

```cpp
if (v[m] > target) {
  r = m;
} else {
  l = m;
}
```

to

```cpp
if (v[m] >= target) {
  r = m;
} else {
  l = m;
}
```

In such way, if we find `v[m] == target`, we will not stop. Instead we will search the left part while keeping `v[r] == target`. When `l == r - 1`, we just check `v[l]` and `v[r]` and see if `v[l] == target`, which is unlikely in this case, or `v[r] == target`. We return the index of the leftmost one that equals to target. By doing this, we find the leftmost target.

The way of finding the rightmost target is similar and is shown in the follwing code.

{% gist 6f158096ca48efc9fb19 %}

## Problems on Rotated, Sorted Arrays

### Search in Rotated Sorted Array
### Search in Rotated Sorted Array II
### Find Minimum in Rotated Sorted Array
### Find Minimum in Rotated Sorted Array II
### Find Peak Element
## Other Binary Search Problems
### Search A 2D Matrix