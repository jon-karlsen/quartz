---
title: Two-Pass
tags:
    - algorithms
    - patterns
    - techniques
---

## What
The two-pass technique unsurprisingly involves traversing a given data structure twice. The first pass is typically use to calculate information that simplifies the problem, and the second pass leverages that information to solve the problem. 

This technique can help avoid intricate logic and improve clarity, although at a cost to time and space complexities. Typically, if each pass is `O(n)`, the algorithm overall will also be `O(n)`.

## When
- problem is best solved by pre-processing data before starting "main" computation/analysis
- problem involves constraints that are more easily handled by analysing the data first in one direction, then subsequently solving or refining it in the opposite

## How
```python
"""
Find the distance from each character in a string `s` to a given target character (`target`). For example, given the string 'EXPONENT', we want to find how far each letter is from a letter 'E'.
"""

s = "EXPONENT"
target = 'E'
n = len(s)
distances = [float('inf')] * n

prev = float('inf')
for i in range(n):
    if s[i] == target:
        prev = i
    if prev != float('inf'):
        distances[i] = i - prev

prev = float('inf')
for i in range(n - 1, -1, -1):
    if s[i] == target:
        prev = i
    if prev != float('inf'):
        distances[i] = min(distances[i], prev - i)
```
