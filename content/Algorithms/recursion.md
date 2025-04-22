---
title: Recursion
tags:
    - concepts
    - algorithms
    - recursion
    - dfs
    - bfs
    - depth-first-search
    - breadth-first-search
---

## tl; dr
_Recursion_ is a function calling itself to solve a problem. In other words, it entails breaking an overall problem into smaller sub-problems.

A recursive function must have a base case to terminate the recursion. Each recursive call processes a sub-problem, and eventually, all calls reach the base case, and the solution is built back up.

Example:
```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n-1)
```

## Advantages
- **elegance:** Some complex problems can be expressed more simply in terms of recursion
- **readability:** Recursive algorithms can potentially be shorter and clearer (e.g., compared to iterative algorithms)
- **natural suitability:** Recursion is naturally suited to problems like tree traversal

## Disadvantages
Compared to e.g. an iterative approach, recursion is typically characterised by:
- higher **memory usage**, as each recursive call adds to the [[call stack]]
- lower **performance** due to the function call overhead
- more difficult **debugging and tracing** 
- a more challenging **mental model**