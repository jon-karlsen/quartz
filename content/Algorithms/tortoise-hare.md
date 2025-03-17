---
title: Tortoise & Hare/Fast & Slow pointer
tags:
    - algorithms
    - techniques
    - patterns
    - fast-and-slow
    - tortoise-and-hare
---

## What
This technique provides an efficient way to detect cycles with `O(n)` time, and `O(1)` space complexities.

## When
- problem involves a [[linked list]] or [[sequence]]
- problem requires determining whether a cycle exists in the structure
- problem requires finding the middle of a [[linked list]]
- problem requires solution to run in `O(1)` time

## How
Initialise two pointers starting from the head of the list/sequence. The slow pointer moves one step at a time, the fast pointer two. If the fast pointer ever catches up to the slow pointer, we have evidence of a cycle. EG.:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

```python
def has_cycle(head: ListNode) -> bool:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

```python
def find_middle(head: ListNode) -> ListNode:
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```