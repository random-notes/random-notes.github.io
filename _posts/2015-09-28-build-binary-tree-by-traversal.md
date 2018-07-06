---
layout: post
title:  "Construct Binary Tree Using Traversal"
date:   2015-09-28
categories: coding
tags: leetcode binary-tree
---

First let’s check an example

```
      7
   /     \
 10       2
 /\       /
4  3     8
    \   /
     1 11
```

The traversal results are as follow:

```
Index:   0  1  2  3  4  5  6  7
In:      4 10  3  1  7 11  8  2
Pre:     7 10  4  3  1  2  8  11
Post:    4  1  3 10 11  8  2  7
```

# Construct a binary tree using inorder and preorder traversal

It is clear that the first element in preorder traversal is the root of this tree, so we divide the inorder vector using the root.

```
Index:    0   1  2  3  4  5  6  7
In:      [4  10  3  1] 7[11  8  2]
Pre:     [7] 10  4  3  1  2  8  11
```

As we can see, `[4,10,3,1]` are in the left subtree of root and `[11,8,2]` are in the right subtree of root. Therefore, we can do the `find root->get left and right subtree` procedure recursively. However, a challenge here is how to determine the root of left and right subtree.

To solve the above problem, we need to go back to the tree representation and recall the way that preorder traversal is done.

```
        root    left           right
Pre:     [7] [10  4  3  1]  [2  8  11]
```

If we divide the result of preorder traversal into three parts, we can see the root is followed by a left subtree and right subtree, so if we can know the size of left subtree, we can know the preorder traversal of right subtree, which is `rootPosition + leftSubTreeSize + 1`. Luckly by finding the position of `7` in inorder traversal, we can know the size of left subtree (and the right subtree also). Note that `[10,4,3,1]` and `[2,8,11]` are also preorder traversal result, which means the first element in these two vector are the root of them respectively.

To implement this algorithm, we need three indexes, first one is `rootPos`, which denotes the index of the root of current subtree, `lpos` and `rpos` denote the range of `rootPos`‘s decedents (not children, which are directly connected). We can find `lpos` and `rpos` by finding the root in inorder, and by fiding the root in inorder traversal, the size of two subtrees can be determined and therefore we can know the root of these two subtress in preorder traversal.

The code are as follow:

{% gist e7aa1e158eeb1f159989 %}

# Construct a binary tree using inorder and postorder traversal

The difference between preorder and postorder traversal is that whether we visit the root at first or at last.

```
Index:   0  1  2  3  4   5  6  7
In:     [4 10  3  1] 7 [11  8  2]
Post:    4  1  3 10 11   8 2  [7]
```

Therefore all we need to do is change the way we find the root of subtrees, namely we do it from right to left by minus the size of right tree.

The code are as follow:

{% gist 8d6dc93141e0466474ea %}