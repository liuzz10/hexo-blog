---
title: LC-121 Best Time to Buy and Sell Stock
date: 2021-03-03 07:07:07
tags:
- leetcode
- interview
---
# Problem

[Puzzle link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

# Idea & Explanation

Fixing a buying day (price A), scan every day after it (price B):
* If the buying day price A is smaller or equal than price B, calculate profit and update max-profit, and keep scanning. We don't change the buying day.

* If the buying day price A is larger than price B, that means, price B can replace price A as a new buying day. Because for any price after price B, it must have a better profit if we buy at price B than price A. Therefore, we update the buying day.

# Code
Python:
```python
def maxProfit(self, prices: List[int]) -> int:
    max_p = 0
    buy = prices[0]
    for i in range(1, len(prices)):
        if buy <= prices[i]:
            profit = prices[i] - buy
            max_p = max(max_p, profit)
        else:
            buy = prices[i]
    return max_p
```
Java:
```java
public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;
        for (int i = 0; i < prices.length; i++) {
            minPrice = Math.min(minPrice, prices[i]);
            maxProfit = Math.max(maxProfit, prices[i] - minPrice);
        }
        return maxProfit;
 }
```

Note: This article was originally published [here](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/discuss/1025235/Python-or-One-pass-or-Explanation-to-beginners).