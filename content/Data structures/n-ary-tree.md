---
title: N-ary tree
tags:
    - tree
    - data-structure
---
## tl; dr
An _n-ary tree_ is a [[tree]] in which each node has no more than `n` children. For example, a binary tree has at most two children (`n = 2`).

#### Implementation
```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
```
### Full Binary Tree
A _full binary tree_ is a binary tree whose every node has either `0` or `2` children.

### Complete Binary Tree
A `complete binary tree` is a binary tree whose every level (except possibly the last) is completely filled and where every node in the last level is as far left as possible.

### Perfect Binary Tree
A _perfect binary tree_ is a binary tree whose every internal node has two children, and whose every leaf node has the same level. Important properties of a perfect binary tree:

1. The number of nodes is `2^n-1`, where `n` is the number of levels. This is a [[geometric sequence]] whose sum is `a(1-r^n)/(1-r)`. Given `a = 1` and `r = 2`, `2^n-1`. 
2. The number of internal nodes is `n - 1`, where `n` is the number of leaf nodes.
3. The total number of nodes is `2 * n - 1`, where `n` is the number of leaf nodes.

### Binary Search Tree
A _binary search tree_ is a binary tree in which every node follows the ordering property `every left descendant < node < every right descendant`.

### Balanced Binary Tree
A _binary search tree_ is a binary tree where the difference between the heights of the left and right subtrees is not more than `1`. Searching, insertion, and deletion in a balanced binary tree take `O(log n)` time (vs. `O(n)` in an unbalanced binary tree).

Common types of balanced binary trees are [[red-black trees]] and [[AVL trees]].