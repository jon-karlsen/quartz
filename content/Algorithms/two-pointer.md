---
title: Two-Pointer
tags:
    - patterns
    - array
    - two-pointer
---

## What
Use two pointers to traverse the data structure. The pointers can move toward each other, or in the same direction. This pattern simplifies many problems that require searching, comparing, or processing pairs of elements by reducing redundant operations, often resulting in an `O(n)` solution.

## When
- problem involves already sorted data, especially in [[array]] or string form
- problem requires finding or comparing pairs of elements
- problem requires computing a result based on interactions between two sections of ata
- problem requires an efficient, often `O(n)` solution

## Why
Avoids nested iteration by moving pointers independently.

## How

```python
"""
Given a sorted array, find two numbers that sum to a target value.
"""
nums = [1, 2, 3, 4, 6]
target = 6
left, right = 0, len(nums) - 1

while left < right:
    current_sum = nums[left] + nums[right]
    if current_sum == target:
        print(f"Pair found: ({nums[left]}, {nums[right]})")
        break
    elif current_sum < target:
        left += 1
    else:
        right -= 1
```

```python
"""
Remove duplicates in-place and return length of modified array.
"""
nums = [0, 0, 1, 1, 1, 2, 3, 3, 4]
if not nums:
    print(0)

i = 0
for j in range(1, len(nums)):
    if nums[j] != nums[i]:
        i += 1
        nums[i] = nums[j]
print(i + 1)
```