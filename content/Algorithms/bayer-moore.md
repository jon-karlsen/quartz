---
title: Bayer-Moore Majority Voting algorithm
tags:
    - array
---

**Time complexity:** `O(n)` <br/>
**Memory complexity:** `O(1)`

An efficient way to find the _majority element_ in an [[array]]. A majority element is an element that appears `> n/2` times in an array of size `n`.

## Steps
1. Initialization:
	- Candidate element (initially `0`)
	- Counter (initially `0`)
2. First pass (find a candidate):
    - Iterate array:
        - If counter is `0`, set current element as candidate
        - If the current element is equal to the candidate, increase the counter
        - If the current element is different, decrease the counter
3. Second pass (verification)<sup>1</sup>:
    - Since the first pass only **finds a candidate**, a second pass counts how many times this candidate appears.
    - If it appears more than **⌊n/2⌋ times**, it’s the majority element; otherwise, there's no majority element.

## Implementation (no verification)
```go
func majorityElement(nums []int) int {
    candidate, count := 0, 0
    for _, num := range nums {
        if count == 0 {
            candidate = num
        }
        if candidate == num {
            count++
        } else {
            count--
        }
    }

    return candidate
}
```

## Implementation (with verification)

```go
func majorityElement(nums []int) int {
    candidate, count := 0, 0

    for _, num := range nums {
        if count == 0 {
            candidate = num
        }
        if candidate == num {
            count++
        } else {
            count--
        }
    }

    count = 0
    for _, num := range nums {
        if num == candidate {
            count++
        }
    }

    if count > len(nums)/2 {
        return candidate
    }

    return -1
}
```
—
<sup>1</sup> Only necessary if the majority element is not guaranteed to exist