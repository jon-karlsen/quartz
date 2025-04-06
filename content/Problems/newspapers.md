---
title: Newspapers
tags:
    - binary-search
    - problem
    - leetcode
---

# Newspapers

You've begun your new job to organize newspapers. Each morning, you are to separate the newspapers into smaller piles and assign each pile to a co-worker. This way, your co-workers can read through the newspapers and examine their contents simultaneously.

Each newspaper is marked with a read time to finish all its contents. A worker can read one newspaper at a time, and, when they finish one, they can start reading the next. Your goal is to minimize the amount of time needed for your co-workers to finish all newspapers. Additionally, the newspapers came in a particular order, and you must not disarrange the original ordering when distributing the newspapers. In other words, you cannot pick and choose newspapers randomly from the whole pile to assign to a co-worker, but rather you must take a subsection of consecutive newspapers from the whole pile.

What is the minimum amount of time it would take to have your coworkers go through all the newspapers? That is, if you optimise the distribution of newspapers, what is the longest reading time among all piles?

### Constraints

`1 <= newspapers_read_times.length <= 10^5`

`1 <= newspapers_read_times[i] <= 10^5`

`1 <= num_coworkers <= 10^5`

### Examples

#### Example 1:

#### Input: `newspapers_read_times = [7,2,5,10,8], num_coworkers = 2`

#### Output: `18`

#### Explanation:

Assign first `3` newspapers to one coworker then assign the rest to another. The time it takes for the first `3` newspapers is `7 + 2 + 5 = 14` and for the last `2` is `10 + 8 = 18`.

#### Example 2:

#### Input: `newspapers_read_times = [2,3,5,7], num_coworkers = 3`

#### Output: `7`

#### Explanation:

Assign `[2, 3]`, `[5]`, and `[7]` separately to workers. The minimum time is `7`.

## Intuition
We are tasked to find the minimum time required to read all papers `k` given `n` coworkers. 

The search space for `k` is `[i, j]`, where:
- `i = max(newspaper_read_count)`: someone has to read the longest paper
- `j = sum(newspaper_read_count)`: if we only have one coworker, s/he has to read all papers

Importantly, the range `[i, j]` is [[monotonic]]. If a given minimum time `k` is valid, then any value `>= k` must also be valid and can therefore be discarded--thus, this problem is [[binary search#Identifying the boundary in a boolean array]], and the remaining challenge is to identify the condition for a valid `k` value.

```python
def newspapers(papers: List[int], coworkers: int) -> int:
    l, r = max(papers), sum(papers)
    min_req_time = 0

    while l <= r:
        m = (l + r) // 2
        if valid(...):
            min_req_time = m
            r = m - 1
        else:
            l = m + 1

    return min_req_time
```

To identify a valid `k` value, we need to verify whether our `n` coworkers can read every paper `p` within the time limit. Or, in other words, given a time limit `k`, is the number of coworkers required to get through all papers less than or equal to our actual number of coworkers?

We'll iterate through the papers, keeping track of the reading time of the current worker, and increasing the required number of workers every time the current worker would not have sufficient time to finish the top paper of the stack. After looking through all papers, we check to see if an additional worker was required.

```python
def valid(papers: List[int], coworkers: int, limit: int) -> bool:
    curr_time, req_workers = 0, 0

    for p in papers:
        if curr_time + p > limit:
            curr_time = 0
            req_workers += 1
        curr_time += p

    if curr_time != 0:
        req_workers += 1

    return req_workers <= coworkers
```

The `if curr_time != 0` conditional can be somewhat counter-intuitive, but really we're just checking whether there's still work in progress after processing all papers--if there is (`curr_time != 0`), the last worker has started but not completed their assigned papers, so we need to count them with `req_workers += 1`.

`curr_time == 0` is an edge case that could occur if the input list `papers` is empty (no papers to process), or if all papers had a processing time of `0`. Essentially, it should never occur with reasonable input. 
