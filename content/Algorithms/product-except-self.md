---
title: Product (Except Self)
tags:
    - array
---


## Problem
Given an [[array]] `nums`, return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

### Example
- Input: `[1, 2, 3, 4]`
- Output: `[24, 12, 8, 6]`

## Steps

### #1: Calculate Prefix Products
Create an array `prefix_products` where each element at index `i` is the product of all elements before `i`.

```
nums:             [ 1,   2,   3,   4 ]

prefix_products:  [ 1,   1,   2,   6 ]
                  [ 1, 1*1, 1*2, 1*2*3 ]
```

### #2: Calculate Suffix Products
Create an array `suffix_products` where each element at index `i` is the product of all elements after `i`.

```
nums:             [ 1,   2,   3,   4 ]

suffix_products:  [ 24,  12,  4,   1 ]
                  [ 2*3*4, 3*4, 4, 1 ]
```

### #3: Combine Prefix and Suffix Products
Multiply the corresponding elements of `prefix_products` and `suffix_products` to get the final `output` array.

```
prefix_products:  [ 1,   1,   2,   6 ]
suffix_products:  [ 24,  12,  4,   1 ]

output:           [ 24,  12,  8,   6 ]
                  [ 1*24, 1*12, 2*4, 6*1 ]
```

### Final Result
- Output: `[24, 12, 8, 6]`

## Python example

```python
def product_except_self(nums):
    n = len(nums)
    if n == 0:
        return []

    output = [1] * n
    for i in range(1, n):
        output[i] = output[i - 1] * nums[i - 1]

    suffix_product = 1
    for i in range(n - 1, -1, -1):
        output[i] = output[i] * suffix_product
        suffix_product *= nums[i]

    return output
```