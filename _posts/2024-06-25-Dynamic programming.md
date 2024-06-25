---
layout: post
title: "Dynamic programming"
date: 2024-06-25
categories: [Algorithms]
tags: [Algorithms, Dynamic programming]
---

Dynamic Programming (DP) is a powerful algorithmic technique used for solving optimization problems by breaking them down into simpler subproblems and solving each subproblem just once. The solutions to these subproblems are stored, so that when the same subproblem occurs again, it can be reused instead of recomputed. This approach saves computation time and avoids redundant work.

### Key Concepts

1. **Optimal Substructure**:
    - A problem exhibits optimal substructure if an optimal solution to the problem can be constructed from optimal solutions to its subproblems.
2. **Overlapping Subproblems**:
    - A problem has overlapping subproblems if the same subproblems are solved multiple times.
3. **Memoization**:
    - A top-down approach where solutions to subproblems are stored in a table (usually an array or hash map) to avoid redundant calculations.
4. **Tabulation**:
    - A bottom-up approach where solutions to subproblems are computed iteratively and stored in a table. This approach usually starts with the smallest subproblems and builds up to the solution of the original problem.

### Steps to Solve a Problem Using Dynamic Programming

1. **Characterize the structure of an optimal solution.**
2. **Define the value of an optimal solution recursively in terms of the values of smaller subproblems.**
3. **Compute the value of an optimal solution (typically in a bottom-up fashion).**
4. **Construct an optimal solution from the computed information (if required).**

### Example Problems and Solutions

1. **Fibonacci Sequence**

The Fibonacci sequence is a classic example of dynamic programming. Each term in the sequence is the sum of the two preceding ones.

### Recursive Approach (Inefficient due to overlapping subproblems):

```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

```

### Dynamic Programming Approach (Memoization):

```python
def fibonacci(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo)
    return memo[n]

```

### Dynamic Programming Approach (Tabulation):

```python
def fibonacci(n):
    if n <= 1:
        return n
    fib = [0] * (n + 1)
    fib[1] = 1
    for i in range(2, n + 1):
        fib[i] = fib[i - 1] + fib[i - 2]
    return fib[n]

```

1. **0/1 Knapsack Problem**

Given a set of items, each with a weight and a value, determine the number of each item to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible.

### Dynamic Programming Solution:

```python
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i - 1] <= w:
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1])
            else:
                dp[i][w] = dp[i - 1][w]

    return dp[n][capacity]

```

### Applications of Dynamic Programming

- **Optimization Problems**: Problems where you need to find the best solution among many.
- **Sequence Alignment**: Used in bioinformatics to align DNA sequences.
- **Resource Allocation**: Problems involving the allocation of resources to maximize profit or minimize cost.
- **Pathfinding**: Algorithms like Dijkstra's and Bellman-Ford use dynamic programming principles.
- **Game Theory**: Used to find optimal strategies in games.

### Conclusion

Dynamic Programming is a versatile and powerful technique used to solve complex problems by breaking them down into simpler subproblems and storing their solutions. This approach is highly efficient for problems exhibiting optimal substructure and overlapping subproblems, making it a fundamental tool in computer science and algorithm design.