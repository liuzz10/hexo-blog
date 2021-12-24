---
title: LC-211 Maximal Square
date: 2021-02-09 07:07:07
tags:
- leetcode
- interview
---
# Problem

[Puzzle link](https://leetcode.com/problems/maximal-square/)

Given an m x n binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

# Idea & Explanation
**What I learnt: dp[][] table doesn't have to contain the direct result.**

Normally in a dp problem, the dp table contains everything we need so we can simply return the value in the corner (e.g., `dp[m][n]`).

Therefore, my first attempt is to generate a 2D table where each cell represents "the area of the largest squre up to this point". For example, I managed to use `dp[i][j]` to represent the max area of the squre given an `i * j` matrix. In the end, I can just return `dp[m][n]` as the result.

Sounds good. However, it added too much complexity. Why?

Because by getting and entering the `max` value into `dp[i][j]`, it may erase the pattern of the original matrix. Therefore, I can't trust the dependency anymore.

For example, we are given two different matrix:

matrix 1:
10
01

dp table:
11
12

matrix 2:
11
11

dp table:
11
12

By using my stretegy, resulting dp tables will be the same. Because by getting `max`, I ignore all the defections which should not be ignored.

Therefore, I need a 2D table to honestly record everything. And I maintain a `max` beside the table.

This is what an optimized solution is trying to do. It calculates the `max` of `min`. For the `max` part, it uses a variable to get. For the `min` part, it uses dp table to get. Each cell `dp[i][j]` represents: if I have to use element `matrix[i][j]` to form a squre in the bottom right corner, how large a squre can be. This is hard to think about, but after coming up with it, the rest is easy.

## Code

```python
def maximalSquare(self, matrix: List[List[str]]) -> int:
    r = len(matrix)
    c = len(matrix[0])
    dp = [[0 for x in range(c+1)] for y in range(r+1)]
    res_max = 0
    for i in range(r):
        for j in range(c):
            if matrix[i][j] == '1':
                dp[i+1][j+1] = min(dp[i][j+1], dp[i+1][j], dp[i][j]) + 1
                res_max = max(dp[i+1][j+1], res_max)
    return res_max * res_max
```
