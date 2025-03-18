---
title: Cyclic Sort
tags:
    - algorithms
    - patterns
    - techniques
    - cyclic-sort
---

## What
Efficiently sort a collection of numbers when the elements are within a specific range. Each number should be placed at the index corresponding to its value.

## When
- input contains numbers within a bounded range, IE. `[1..n]`
- problem involves rearranging or finding specific patterns, EG. missing, duplicate, or misplaced numbers
- problems requires solving in constant space

## How
Iterate through the array, and for each element, if the element is not not at the correct index, swap with element at target index.

```python
"""
Given an array of numbers where each number is in the range from 1 to n, sort the array in-place (without using extra space) in O(n) time.
"""
def cyclic_sort(nums):
    i = 0
    while i < len(nums): 
        correct_idx = nums[i] - 1 

        if nums[i] != nums[correct_idx]: 
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
        else:
            i += 1

    return nums
```

```python
"""
Given an unsorted array of integers, find the smallest missing positive number.
"""
def find_smallest_missing_positive(nums):
    n = len(nums)

    i = 0
    while i < n:
        correct_idx = nums[i] - 1
        if 1 <= nums[i] <= n and nums[i] != nums[correct_idx]:
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
        else:
            i += 1

    for i in range(n):
        if nums[i] != i + 1:
            return i + 1

    return n + 1
```