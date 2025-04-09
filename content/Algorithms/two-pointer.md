---
title: Two-Pointer
tags:
    - patterns
    - array
    - two-pointer
---

## What
_Two pointers_ is a technique that uses two pointers to traverse an iterable data structure. The pointers can move toward each other, or in the same direction. This technique simplifies many problems that require searching, comparing, or processing pairs of elements by reducing redundant operations, often resulting in an `O(n)` solution. The two pointers technique also includes [[sliding-window]].

## When
- problem involves already sorted data, especially in [[array]] or string form
- problem requires finding or comparing pairs of elements
- problem requires computing a result based on interactions between two sections of array
- problem requires an efficient, often `O(n)` solution
- problem involves detecting a cycle in a [[linked list]]

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

```python
"""
Find middle of a linked list.

Input: `0 1 2 3 4`

Output: `2`

If the number of nodes is even, then return the second middle node.

Input: `0 1 2 3 4 5`

Output: `3`
"""
class Node:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next

def find_middle(head: Node) -> int:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow.val

```