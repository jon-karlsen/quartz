---
title: Sliding Window
tags:
    - algorithms
    - techniques
    - patterns
    - arrays
    - strings
    - subarrays
    - substrings
---

## What
Examine a sequence of elements that form contiguous subsets. The window can grow, shrink, or remain constant in size, depending on the problem. Rather than recalculating a subdivision for each step, update the window as required.

## When
- problem involves working with contiguous elements
- problem involves subarrays or substrings
- problem requires an `O(n)` solution

## How

```python
"""
Fin the max sum of 3 contiguous elements in an array.
"""
nums = [2, 1, 5, 1, 3, 2]
k = 3
max_sum = 0
current_sum = sum(nums[:k])
for i in range(len(nums) - k):
    current_sum = current_sum - nums[i] + nums[i + k]
    max_sum = max(max_sum, current_sum)
```

```python
"""
Find longest substring without repeating characters.
"""
s = "abbcab"
char_set = set()
left = 0
max_len = 0
for right in range(len(s)):
    while s[right] in char_set:
        char_set.remove(s[left])
        left += 1
    char_set.add(s[right])
    max_len = max(max_len, right - left + 1)
```