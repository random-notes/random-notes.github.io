---
layout: post
title:  "Partition, Kth Element, and Sort"
date:   2015-10-11
categories: coding
tags: leetcode partition k-element sort
---
# Partition (2-part partition)
It is very often that we need to partition an array or something into two parts so we can do quick sort. The partition of such purpose can be done using the following code:

This code partitions the subarray of `vec[i:j]`, `i` and `j` are inclusive.

{% gist 2ff85e366c2b02938809 %}

Note that at the start of this code snippet we first try if the pivot is the starting index of this subarray, which means if the subarray only contains two elements. It is straightforward to prove this because `floor((i+j)/2)=i`, then `i+j = 2*i (+0.5)`, therefore `j = i` or `j = i + 1`. In this case we can not execute the normal procedure because it will have out-of-bound problem.

```
  0 1
  ^ ^
  i j

  0 1
  ^
(i,j)

  0 1
^   ^
i   j
```

See above example, `while(vec[j] > pivot) --j` will execute until `vec[j] == 0`. At that time, `vec[i] == 0`. Then we swap them two and move `i` and `j` by one, we get `j` points to `1` and `i` points to somewhere out of the boundary.

To avoid this problem, we add that extra procedure that if there are only two elements, do not bother to do the fancy partition just sort them and return the index of pivot.

However, the resulting index that returned by this function is not always the index of pivot. Remember at the very beginning we said that this function partitions an array into two parts, we do not specify that by what number. An example is as follow:

```
*start*

pivot = vec[1] = 0
index: 0  1  2  3
value: -1 0  1 -1
       ^        ^
       i        j

*i-while && j-while*

pivot = vec[1] = 0
index: 0  1  2  3
value: -1 0  1 -1
          ^     ^
          i     j

*swap*

pivot = vec[1] = 0
index: 0  1  2  3
value:-1 -1  1  0
             ^
           (i,j)

*i-while && j-while*

pivot = vec[1] = 0
index: 0  1  2  3
value:-1 -1  1  0
          ^  ^
          j  i

*stop*
```

In this case, the resulting partitions are `-1,-1` and `1,0`. Although all elements in left part are smaller than things in the right part, the partition is not based on `pivot = 0`. To make a partition that `left < pivot < right`, we need a 3-part partition instead of this basic 2-part partition.

# 3-Part Partition

Note in the previous section we mentioned that the partition used in quick sort is not a 3-part partition which means not all the elements in the left part is smaller than the pivot and all the elements in the right part is larger than the pivot. This is fine for quick sort since the ultimiate goal of quick sort is sort the entire array and we do not really care about this extra feature. But if we want to explicitly find an item, or say find kth element (it can be the largest or the smallest), we need a 3-part partition.

A sample code is described as follow:

{% gist a297cdc2ca264ca20b49 %}

The key idea of this function is, we keep three indexes, `l` means a boundary that all elements before `l` are smaller than `pivot`; `r` means a boundary that all elements after `r` are larger than `pivot`.

At the beginning of this function we do not know anything about the comparison result so `l` and `r` is the original boundary of this input subarray.

Then we start compare the elements, every time we find a smaller element, we switch it with `vec[l]` because `l` is the successor of a sequence of smaller elements, then we increase `k` and `l` by one because 1) `l` is no longer the boundary, `l+1` is, and 2) the element we switched to `k` is equal to pivot so we start on next.

If we find an element that is larger than pivot, we switch it with `r`. Note here we only decrease `r` by `1` because `r-1` now is the new boundary, but we have not examed `vec[r]`, which now is at `vec[k]`, so we need to exam it first before we move to next index. In previous paragraph we move `k` and `l` at the same time because every element between `start ~ k` have beeen compared and we know that there are no element that is larger than `pivot`.

This 3-part partition can have some interesting usage except find kth element. For example if we have a limited value range of elements and we want to do a in place sort, we can use partition sort by setting the pivot as all the elements but the smallest and the largest one. Say we have an array of elements that has value 1,2,3,4, we can set the pivot to 2,3 so after running the aforementioned algorithm, we can put all 1s at the beginning of the array and all 4s at the end of the array. then we repeat the same procedure by sorting 2,3 by pivot 2.5 in subarray `vec[l:r]`.