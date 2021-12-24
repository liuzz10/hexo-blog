---
title: LC-78 Subsets
date: 2021-01-19 07:07:07
tags: 
- leetcode
- interview
---
# Problem

[Puzzle link](https://leetcode.com/problems/subsets/)

# TL;DR

The idea is that, when we are looking at each number, we have 2 options: 
* include it, or
* not include it

The graph for this algorithm is a binary tree where the left branch represents "with" this number and the right branch represents "without" this number. Therefore no for loop is needed.

```python
def subsets(self, nums: List[int]) -> List[List[int]]:
    res = []
    def dfs(index, path):
        if index == len(nums):
            res.append(path)
            return
        dfs(index + 1, path + [nums[index]]) # with nums[index]
        dfs(index + 1, path) # without nums[index]

    dfs(0, [])
    return res
```

Note: I'd not call this approach as backtracking because by using backtracking we usually have to prune partial solutions according to the definition on [wiki](https://en.wikipedia.org/wiki/Backtracking#:~:text=Backtracking%20is%20a%20general%20algorithm,possibly%20be%20completed%20to%20a). In this problem, it doesn't prune any partial solutions - it just simply includes all combinations.

# Origin

Why came up with this solution? Actually it's not very hard to think of if you have considered how many possible subsets in total. Given a list of length of n, the answer is 2^n. Because for every element, there are two options: to include it in the subset or not. Therefore there will be in total 2*2 *...* 2*2 subsets.

When we see the pattern like 2^n, we think of binary tree/dfs/backtracking...whatever you call it, and every single leave will be a unique subset. The left-most leaf is when we include all numbers so it will be a full list, while the right-most leaf is when we exclude all numbers so it will be an empty list.

The patter of the binary tree reminds us to use dfs. That's how I got started at least.

# Another way to code

Simply replace `return` with an `if...else...` statement. By using either `return` or using `else`, we garentee that it will be able to terminate the recursion. If `return` or `else` is missing, it will call `dfs()` forever and never ends, which is bad.

```python
def subsets(self, nums: List[int]) -> List[List[int]]:
    res = []
    def dfs(index, path):
        if index == len(nums):
            res.append(path)
        else:
            dfs(index + 1, path + [nums[index]]) # with nums[index]
            dfs(index + 1, path) # without nums[index]

    dfs(0, [])
    return res
```

# Common mistakes

path.append() returns `None` so the `path` argument in `dfs(index, path)` will be `None` when it's get called.

```python
# This is wrong
def subsets(self, nums: List[int]) -> List[List[int]]:
    res = []
    def dfs(index, path):
        if index == len(nums):
            res.append(path)
        else:
            dfs(index + 1, path.append(nums[index])) # this returns None
            dfs(index + 1, path)

    dfs(0, [])
    return res
```

Note: This article was originally published [here](https://leetcode.com/problems/subsets/discuss/955057/DFS-or-Very-simple-Python-solution-NO-FOR-LOOP-needed).
