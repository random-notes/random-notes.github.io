---
layout: post
title:  "Ugly Number"
date:   2015-08-19
categories: coding
tags: leetcode
---
# Ugly Number and Find $n^{th}$ Ugly Number
The question description can be found at Leetcode: (263) Ugly Number.

The definition of **ugly number** is the prime fractors of a number only contains `2`,`3`,`5` (`1` is also an ugly number and it is an exception). Therefore a list of ugly numbers are

```
index:   1  2  3  4  5  6  7  8  9  10 ...
number:  1  2  3  4  5  6  8  9  10 12 ...
```

The first problem, *263 Ugly Number* gives an integer as input and the output is whether this is an ugly number or not. In order to solve this problem, we need to know how the ugly numbers are constructed.

Namely any ugly number can be written as $2^n \times 3^m \times 5^k$, therefore to validate an ugly number, all we need to do is divide that number by `2`,`3`, and `5` and see if there is any fraction.

{% gist 999c226fceb59cc8c549 %}

# Find $n^{th}$ Ugly Number

The question description can be found at Leetcode: (264) Ugly Number II.

This is a follow up problem of the aforementioned ugly number problem. It is easy to validate an ugly number but to generate $n^{th}$ ugly number needs some effort.

Since we know any ugly number can be represented by $2^n \times 3^m \times 5^k$, we denote this as `(n,m,k)`. Therefore, the pervious ugly number list becomes

```
index:     1        2        3        4        5        6        7        8        9       10   ...
number:    1        2        3        4        5        6        8        9       10       12 ...
      : (0,0,0)  (1,0,0)  (0,1,0)  (2,0,0)  (0,0,1)  (1,1,0)  (3,0,0)  (0,2,0)  (1,0,1) (2,1,0) ...
```

If we maintain three ordered queues, the first queue multiple 2 to every new element pushes into it, the second and third multipe 3 and 5 respectively, we can somehow get the result by merging these three queues. The problem is, how should we construct the queues?

An intuitive idea is that a ugly number `(a,b,c)` can construct three future ugly numbers `(a+1,b,c)`, `(a,b+1,c)`, and `(a,b,c+1)`. If `a=1`, `b=0`, `c=0`, the three ugly numbers are `(2,1,1)`, `(1,2,1)`, and `(1,1,2)`. Unfortunately, although we know `(2,0,0) < (1,1,0) < (1,0,1)`, we do not know how many other ugly numbers lies between them based on the information we have so far. The good news is, for `(a',b',c') > (a,b,c)`, we know that `(a'+1,b',c') > (a+1,b,c)`, `(a',b'+1,c') > (a,b+1,c)`, and `(a',b',c'+1) > (a,b,c+1)`. That means if we keeping multiplying a new ugly number by 2, 3, 5 and push them into each queue, the numbers in each queue are always in order.

Let’s do an example to illusrate this:

At the beginning, each queue only contains only element 1

```
nth: 0
val: unknown

queue2: 1
queue3: 1
queue5: 1
```

We check the front element of each queue and remove the smallest one, which is 1, and then we construct three new numbers and put them into three queues.

Note that we removed all 1s in three queues.

*Embedded numbers mean already removed*

```
nth: 1
val: 1

queue2: [1] 2
queue3: [1] 3
queue5: [1] 5
```

Then we check the fronts again and remove 2, then we construct three more ugly numbers using 2.

```
nth: 2
val: 2

queue2: [1] [2] 4
queue3: [1]  3  6
queue5: [1]  5  10
```

and so on.

```
nth: 3
val: 3

queue2: [1] [2] 4 6
queue3: [1] [3] 6 9
queue5: [1]  5 10 15
```

{% gist 60898b3551ffa7af70ad %}