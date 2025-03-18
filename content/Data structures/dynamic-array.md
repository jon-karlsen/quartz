---
title: Dynamically-sized array
tags:
    - data-structures
    - arrays
    - dynamically-sized-arrays
---

## What
Where a simple [[array]] has a static capacity that must be allocated up-front, modern languages also support _dynamically-sized arrays_ which automatically adjust their capacity up or down by allocating a new copy of the array as needed. These operations are costly, but dynamically-sized arrays guarantee better _amortised performance_ by performing them only when necessary.

| | Avg. | Worst |
| --- | --- | --- |
| Space | `O(1)` | |
| Index | `O(1)` | |
| Search | `O(n)` | |
| Append | `O(1)` | `O(n)` |
| Insert | `O(n)` | |
| Delete | `O(n)` |