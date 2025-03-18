---
title: Array
tags:
    - data-structure
    - array
---

An _array_ is a linear data structure that stores elements of the same type in contiguous memory locations. It is generally allocated upfront and limited to its allocated capacity. Each element is accessed using an _index_, typically starting from `0`.

Since each element's location in memory in known, read and write operations are `O(1)`. Removal, insertion, and searching operation can be `O(n)`. 

| | Avg. & Worst |
| --- | --- |
| Space | `O(1)` |
| Index | `O(1)` |
| Search | `O(n)` |
| Append | `O(1)` |
| Insert | `O(n)` |
| Delete | `O(n)` |

## When to use?
- need data in an ordered list
- need fast indexing
- limitations on memory

## When to avoid?
- need to efficiently search for unsorted items
- frequently need to insert and/or remove items

## Common operations
- Insert
- Remove
- Update
- Find
- Iterate
- Copy (entire or part)
- Sort
- Reverse
- Swap two items
- Filter
