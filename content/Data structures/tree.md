---
title: Tree
tags:
    - data-structure
    - tree
    - binary-tree
---

## tl; dr
A _tree_ is a [[graph data structure]] composed of _nodes_ and _edges_ that connect these nodes in a parent-child relationship. A node is sometimes also called a _vertex_.

A tree is _acyclic_, and a path exists from the root to any other node. There exist `n-1` edges (with `n` being equal to the number of nodes in the tree), and except for the root, every node has exactly one parent node.

## Terms
- **Internal node:** a node that has at least one child node
- **Leaf node:** a node that has no child nodes
- **Ancestor:** any node that lies on the path from the root to a particular node (exclusive)
- **Descendant:** any node that is reachable from the current node when moving _down_ the tree
- **Level:** the number of ancestors from the root to the current node

## Advantages
Trees are naturally well-suited to represent _hierarchical data_ such as file systems, because they clearly express node relationships. Many trees offer fast searching, insertion, and deletion, and some trees also self-balance to maintain performance.

## Disadvantages
Trees are more complex than linear data structures, and storing parent-child relationships impacts memory usage. Different traversal methods are suited for different uses, and the self-balancing properties of some trees come with some overhead.