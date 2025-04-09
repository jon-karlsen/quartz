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

## Fixed Sliding Window
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