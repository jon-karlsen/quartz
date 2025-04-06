---
title: Binary Search
tags:
    - algorithms
    - binary-search
    - sorted-array
    - divide-and-conquer
    - divide-conquer
---

_Binary search_ is an efficient algorithm for finding a specific element in a sorted collection. It is also a powerful technique to solve problems that can be reframed as searching within a [[monotonic]] space, even when the search space isn't explicitly represented as an array. Its key characteristic is the halving of the search range for every step. It has a time complexity of `O(log n)`, and memory complexities of `O(1)` and `O(log n)` for iterative and recursive implementations, respectively.

The precondition for binary search is to find a monotonic predicate `f(x)` that returns either `true` or `false` based on the problem constraints. Once the predicate is identified, the problem becomes [[#Identifying the boundary in a boolean array]].

## Vanilla Implementation (Python)
```python
def binary_search(nums: List[int], target: int) -> int:
    l, r = 0, len(nums) - 1

    while l <= r:
        mid = (l + r) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            l = mid + 1
        else:
            r = mid - 1
    
    return -1
```

## Identifying the boundary in a boolean array

Given an array of boolean values sorted with all `false` values to the left and all `true` values to the right, find the index of the first element whose value is `true`. If there is no such element, return `-1`.

### Intuition
While keeping track of the lowest `true` index we've seen so far:
1. Examine the middle element; if it is `true`, update the lowest seen index and discard the right half of the array
2. Else, discard the left half of the array

### Implementation
```python
def binary_search(arr: List[bool]) -> int:
    l, r = 0, len(arr)-1
    boundary_idx = -1

    while l <= r:
        mid = (l + r) // 2
        if arr[mid]:
            boundary_idx = mid
            r = mid - 1
        else:
            l = mid + 1

    return boundary_idx
```

### Notes
Many binary search-related problems boil down to finding the boundary in a boolean array.

## Notes
`<=` is used for `while` condition because in the case of `l` and `r` pointing to the same element, `<` would miss it.

When calculating `mid`, if the length of the array is an even number there will be two elements in the middle. The convention is to pick the first of the two, equivalent to performing integer division. In most languages we must take care to avoid integer overflow: `mid = l + floor((r + l) / 2)`, but this is a non-issue in Python as Python allows integers to be arbitrarily large.