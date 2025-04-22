---
title: Prefix Sum
tags:
    - algorithms
    - techniques
    - patterns
    - arrays
    - prefix-sum
---
_Prefix sum_ is a technique whose primary purpose is to allow for constant-time range sum queries on an array. The prefix sum of an element at `array[i]` is the sum of all numbers `[0:i]`.

**Common problems:**
- find continuous sub-array whose sum match target value
- locate sub-arrays that sum to zero
- longest sub-array where `sum = k`
- smallest sub-array where `sum > x`
- maximum sum of fixed-size sub-arrays

_Suffix sum_ is similar in concept, but calculated from the end such that an element at `array[i]` is the sum of all numbers `[-1:i]`.

### vs. Sliding Window
Both [[sliding window]] and prefix sum solve problems related to sub-arrays, but they have different use cases:

- **prefix sum** is ideal for quick range sum queries on static arrays
- **sliding window** is better for problems involving continuous sub-arrays with specific conditions or sizes

### Implementation
```python
def prefix_sum(arr: List[int]) -> List[int]:
    prefix_sum = [0]
    for num in arr:
        prefix_sum.append(prefix_sum[-1] + num)
    return prefix_sum
```

## Sub-array Sum
Given an array of integers and an integer `target`, find a subarray that sums to `target` and return its start and end indices.

Input: `arr: 1 -20 -3 30 5 4` `target: 7`

Output: `1 4`

Explanation: `-20 - 3 + 30 = 7`. The indices for subarray `[-20,-3,30]` is `1` and `4` (right exclusive).

### Implementation
```python
def subarray_sum(nums: List[int], target: int) -> List[int]:
    prefix_sums = {0: 0} # Already seen prefix sums w/ indices
    curr_sum = 0

    for i in range(len(nums)):
        curr_sum += nums[i]
        complement = curr_sum - target
        if complement in prefix_sums:
            return [prefix_sums[complement], i + 1]
        prefix_sums[curr_sum] = i + 1
    return []
```

#### tl; dr
We want to find sub-sections of the list that sum to `target`. As we iterate over the list, we keep a count of the cumulative sum. For each step, the magic question is how many of whatever unit we're counting we'd need to drop from that sum to exactly match `target`--if that exact number exists at a previous point, we've identified a valid sub-array and can return its boundaries.

#### Analysis

The sum of a sub-array `[i, j]` is equal to the sum of `[0, j]` minus the sum of `[0, i - 1]`, so we keep a running prefix sum from which we at each iteration subtract `target` to find the [[complement]] to the current sum (in effect the lower boundary of the sub-array). If we've already seen that complement, we have a valid sub-array and can return its indices.

Note that the `complements` dictionary is initialised with `{0: 0}`, which in terms of the indices we'll be working with from here on means that said indices are effectively 1-based.

Note also that in this implementation, `prefix_sums[i]` would be the sum of `[0..i)` (IE. a half-open interval). This has the advantage of `prefix_sum[0]` denoting the sum of an empty array (IE. the sum of the array up to, but not including the first element). This is useful when there exists a sub-array starting from 0 that sums up to the target--without this definition, we'd not be able to find the sub-array because its complement `0` does not exist in the dictionary.

### Follow-up: Sub-Array Sum Equals Target (Multiple)
```python
def subarray_sum_total(arr: List[int], target: int) -> int:
    prefix_sums: Counter[int] = Counter() # Frequency of each sum
    prefix_sums[0] = 1 # empty array sums to 0
    count, curr_sum = 0, 0

    for val in arr:
        curr_sum += val
        complement = curr_sum - target
        if complement in prefix_sums:
            count += prefix_sums[complement]
        prefix_sums[curr_sum] += 1

    return count
```
#### tl; dr
As opposed to [[#Sub-array Sum]], we now want to find _the total count_ of sub-arrays that sum to `target`. As before, we keep a count of the cumulative sum, and the question remains the same--how many (EG.) bananas would we have to discard to have exactly `target`? Again, if we've already seen that number, we've identified a valid sub-array and can increment the count of such. We do that by adding the number of sub-arrays that end at the current index.

## Product of Array Except Self
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

Input: `[1, 2, 3, 4]`.

Output: `[24, 12, 8, 6]`.

### Implementation
```python
def product_of_array_except_self(nums: List[int]) -> List[int]:
    result = [1] * len(nums)

    left = 1
    for i in range(len(nums)):
        result[i] = left
        left *= nums[i]

    right = 1
    for i in reversed(range(nums)):
        result[i] *= right
        right *= nums[i]

    return result
```

For both passes, we first set `result[i] = left` or `result[i] *= right` multiplying by the current element. This ensures that for both passes, we haven't yet included the current number when setting the product. In other words, the non-inclusive property of this algorithm depends on exactly this order of operations.