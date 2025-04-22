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

### vs. Prefix Sum
Both sliding window and [[prefix sum]] solve problems related to sub-arrays, but they have different use cases:
- **prefix sum** is ideal for quick range sum queries on static arrays
- **sliding window** is better for problems involving continuous sub-arrays with specific conditions or sizes

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

## Fixed-Size Sliding Window
```python
def fixed_sliding_window(input, win_size):
    ans = window = input[0:window_size]

    for right in range(win_size, len(input)):
        left = right - win_size
        remove input[left] from window
        append input[right] to window
        ans = f(ans, window)

    return ans
```

### Find all anagrams in a string

#### Problem
Given a string `original` and a string `check`, find the starting index of all substrings of `original` that is an anagram of `check`. The output must be sorted in ascending order.

#### Parameters

- `original`: A string
- `check`: A string

#### Result

- A list of integers representing the starting indices of all anagrams of `check`.

#### Examples

##### Example 1

Input: `original = "cbaebabacd"`, `check = "abc"`

Output: `[0, 6]`

Explanation: The substring from `0` to `2`, `"cba"`, is an anagram of `"abc"`, and so is the substring from `6` to `8`, `"bac"`.

##### Example 2

Input: `original = "abab"`, `check = "ab"`

Output: `[0, 1, 2]`

Explanation: All substrings with length `2` from `"abab"` are anagrams of `"ab"`.

#### Constraints

- `1 <= len(original), len(check) <= 10^5`
- Each string consists of only lowercase characters in the standard English alphabet.

#### Intuition
Right off that bat, if `check` is longer than `original` the latter can't possibly contain even a single anagram--if such is the case, return an empty list.

For the valid strings `check` and `original`, we will use a fixed-size sliding window to check the validity of each substring, the length of which will equal to that of `check`.

A valid substring is a string for which the frequency of its constituent characters matches that of the constituent characters of `check`. We'll use two fixed-size [[array]]s with capacity 26 to track the frequencies, indexes represent character values, for easy comparison.

We'll initially check the first `len(check)` characters, then slide our window over the remaining substring. For each iteration, decrement the frequency of the character now left behind and increment the frequency of the character newly captured by the window, and check for validity. If valid, add starting index of substring to results array.

```python
def anagram_indexes(original: str, check: str) -> List[int]:
    n_original, n_check = len(original), len(check)
    res = []

    if n_check > n_original:
        return res

    a_ord = ord('a')
    window = [0] * 26
    check_freq = [0] * 26

    for i in range(n_check):
        window[ord(original[i]) - a_ord] += 1
        check_freq[ord(check[i]) - a_ord] += 1

    if window == check_freq:
        res.append(0)

    for right in range(n_check, n_original):
        left = right - n_check

        window[ord(original[left]) - a_ord] -= 1
        window[ord(original[right]) - a_ord] += 1

        if window == check_freq:
            res.append(left + 1)

    return res
```

## Flexibly-Sized Sliding Window (Longest)
Instead of the largest sum among all subarrays of fixed size `k`, we want to find the length of the _longest_ subarray with `<= target`.

Given input `nums = [1, 6, 3, 1, 2, 4, 5]` and `target = 10`, then the longest subarray that does not exceed 10 is `[3, 1, 2, 4]`, so the output is `4` (length of `[3, 1, 2, 4]`).

### Implementation
```python
def subarray_sum_longest(nums: List[int], target: int) -> int:
    longest, win, left = 0, 0, 0

    for right in range(len(nums)):
        win += nums[right]
        while win > target: # while window is INVALID
            win -= nums[left]
            left += 1
        longest = max(longest, right - left + 1)

    return longest
```

### Analysis
The predicate function within the loop is _naturally satisfied_ even before we start processing the array. Growing the window could break this validity--if so, we must shrink the window until it returns to a valid state. That is, any window will be in a valid state both when the outer loop starts and when it terminates. This invariant property allows the update of `longest` at the end of each iteration.

tl; dr to find the longest sub-array, move `left` as little as possible.

## Flexibly-Sized Sliding Window (Shortest)
Given a positive integer array `nums`, find the length of the _shortest_ subarray such that the subarray sum is at least `target`. For input `nums = [1, 4, 1, 7, 3, 0, 2, 5]` and `target = 10`, the smallest window with the `sum >= 10` is `[7, 3]` with length `2`. 

Assume for this problem that it's guaranteed `target` will not exceed the sum of all elements in `nums`.

### Implementation
```python
def shortest_subarr_sum(nums: List[int], target: int) -> int:
    shortest = len(nums)+1
    win, left = 0, 0

    for right in range(len(nums)):
        win += nums[right]
        while win <= target: # while window is VALID
            shortest = min(shortest, right - left + 1)
            win -= nums[left]
            left =+ 1

    if shortest > len(nums): # no valid subarray was found
        return 0
        
    return shortest
```

### Analysis
In contrast to [[#Flexibly-Sized Sliding Window (Longest)]], the initial state is _naturally invalid_ (the starting `win` is not equal to or greater than `target`).

tl; dr to find the shortest sub-array, move `left` as far as possible. Because an empty array is invalid, `left` will never reach or pass `right`.