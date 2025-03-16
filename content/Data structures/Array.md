---
title: Array
tags:
    - data-structure
    - array
---

An _array_ is a linear data structure that stores elements of the same type in contiguous memory locations. Each element is accessed using an _index_, typically starting from `0`.

## Advantages

1. `(O(1))` access to elements using an index.
2. **Memory Efficiency** – No extra overhead per element (unlike linked lists).
3. **Cache-Friendly** – Stored in contiguous memory, making CPU cache access faster.
4. **Easy Sorting & Searching** – Works well with algorithms like binary search `(O(log n))` if sorted.

## Disadvantages

1. **Fixed Size** – Requires pre-allocation or resizing (costly in dynamic arrays).
2. **Expensive Insertions & Deletions** – Adding/removing elements (except at the end) requires shifting `(O(n))`.
3. **Inefficient for Dynamic Operations** – Linked lists are better for frequent inserts/deletes.
4. **Wasted Space (in Dynamic Arrays)** – Dynamic arrays often allocate extra memory for resizing.