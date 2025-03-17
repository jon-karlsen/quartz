---
title: Bit Manipulation
tags:
    - algorithms
    - techniques
    - patterns
---

## What
Work directly with the binary representations of integers. Bitwise manipulation is often faster than arithmetic operations.

## When
- problem requires working with individual bits
- problem requires binary operations
- problem involves integers and requires optimisation of time/space complexity

## How
Common operators:

- **AND (`&`)**: Compares each bit and returns 1 if both bits are 1
- **OR (`|`)**: Compares each bit and returns 1 if at least one bit is 1
- **XOR (`^`)**: Compares each bit and returns 1 if bits are different
- **NOT (`~`)**: Inverts all the bits
- **Left Shift (`<<`)**: Shifts bits to the left, effectively multiplying the number by 2
- **Right Shift (`>>`)**: Shifts bits to the right, effectively dividing the number by 2

> [!warning] Overflow and truncation
> In languages like JavaScript and C, bitwise operations are often limited to 32-bit or 64-bit integers, which can cause unexpected results. For example, shifting 1 left by 33 bits in JavaScript:
> ```js
> let n = 1 << 33; console.log(n);  // Output: 2 (due to 32-bit truncation)
> ```
> Any shift count larger than 31 is wrapped around for a 32-bit integer, leading to potential unexpected results like truncation or overflow.

```python
def is_even(num):
    return (num & 1) == 0
```

```python
def count_set_bits(n):
    count = 0
    while n:
        count += n & 1
        n >>= 1
    return count
```