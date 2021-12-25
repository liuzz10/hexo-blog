---
title: LC-277 Find the Celebrity
date: 2020-08-18 07:07:07
tags:
- leetcode
- interview
---
# Problem

[Puzzle link](https://leetcode.com/problems/find-the-celebrity/)

Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) which tells you whether A knows B. Implement a function int findCelebrity(n). There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return -1.

# Idea 1: Brute Force
For each number i, we need to check if i is a celebrity or not. How to check? We check all knows(i, j) where j from 0 to n. Only when all pairs of knows(i, j) == 0 && all pairs of knows(j, i) ==1, we know i is a celebrity. Otherwise, i is not a celebrity.  

## Time cost
T(n) = O(n^2), since we need to check n numbers, and for each number we need to pair it with n-1 numbers to check. Another way to think about it is to check all n^2 pairs of knows(i, j).

# Idea 2: Linear Elimination
Do we need to check all pairs of knows(i, j)? We don't.

If i knows j, that means i is not the celebrity because a celebrity doesn't know anyone. So we can eliminate i.

If i doesn't know j, that means j is not the celebrity because everyone knows the celebrity. So we can eliminate j.

If you understand the above, you know two properties of celebrity:
* Property 1. If knows(i, j) = 1, i is not the celebrity.
* Property 2. If knows(i, j) = 0, j is not the celebrity.

We will, and only will, get two results: 0 or 1 after calling `knows`. So with each call to `knows(i, j)`, we can eliminate a non-celebrity from n.

## Algorithm
Based on the above analysis, we design the algorithm below:

**Step 1**
Base case: i = 0, j = 1. We check knows(0, 1) and determine a potential candidate.
* If knows(0, 1) = 1, j is the potential candidate.
* If knows(0, 1) = 0, i is the potential candidate.

**Step 2**
After determining a candidate between 0 and 1, we need to compare the winning candidate with a 3rd number. Therefore, we need a for loop to always go to the next number so that the candidate can compare to.
    
For loop where j traverses all numbers:
* `if knows(candidate, j) == 1` we eliminate the current candidate and set `candidate = j`.
* `if knows(candidate, j) == 0` we eliminate j and keep the current candidate.

After running up all numbers, we will get a final candidate X. Also we know that, if a celebrity exists, it must be X.

Here is a tree that demonstrates all possible paths. At each level we will add a new number to compare with, so the height/time cost is theta(n). 

![](LC277.png)

Are we done? No. This final candidate X is just our best guess! Remember to check if X is the real celebrity.

Why? Because in the problem statement, "There will be exactly one celebrity if he/she is in the party...If there is no celebrity, return -1.". It's possible that there's no celebrity.

Since we only compared the final candidate X once, we don't know the relationship between X and other numbers. Since there can be no celebrity, maybe X is not. We trust X just because other numbers are definitely not!

So, we need one more step to check X:

**Step 3**
Check if knows(final candidate, k) == 0 && knows(k, final candidate) == 1. If true, it is the real celebrity that we are looking for.

## Time cost
Therefore, T(n)=O(n). For step 1&2, T(n)=O(n) because the height of the tree is theta(n). For step 3, T(n)=O(n) because we will only need to compare with other n-1 numbers twice, (i, j) and (j, i). 

## Code
```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        // Compare and eliminate one that is not celebrity. Compare with the next number until reaching the last number.
        int candidate = 0;
        for (int j = 1; j < n; j++)
            if (knows(candidate, j)) {
                candidate = j; // We get a final candidate by linear comparison
            }

        // Check if the final candidate is the celebrity
        for (int k = 0; k < n; k++) {
            if (candidate == k) continue;
            if (knows(candidate, k) || !knows(k, candidate)) { // If it knows someone, or someone doesn't know it, it's not a celebrity
                return -1;
            }
        }
        return candidate;
    }
}
```

Note: This article was originally published [here](https://medium.com/@joylovemercy/277-find-the-celebrity-linear-comparison-o-n-step-by-step-explanation-w-graph-8928230c6e)