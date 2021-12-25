---
title: LC-11 Container With Most Water
date: 2021-01-10 07:07:07
tags: 
- leetcode
- interview
---
# Problem

[Puzzle link](https://leetcode.com/problems/container-with-most-water/)

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

![problem](lc-11-container/question.jpg)

# Idea 1: Brute force

1. Calculate the area of all possible containers S(i, j). Here, S(i, j)=min(i, j)*(j-i).
2. We need to use two pointers: i, j. When i=1，j moves from the integer next to i, which is 2, to the end, which is n. Calculate S(i, j)。Then, i moves to the next integer which is 2. j starts from i+1 to n. Repeatedly doing this until i reach n-1.
3. We also need a variable to record the max area among all possible S. It’s easy. We set up and initialize maxArea=-1. If we find any S>max，we assign the value of S to maxArea. Otherwise, we don’t change the value of maxArea so that maxArea always stands for the max value among S.
4. T(n)=O(n^2). One way to think about it is that we have a nested loop (j nested in i). Another way to think about it is “how many possible containers that n lines can make up, if we pick 2 every time”. We use combination to answer this question: the possible combinations in picking 2 lines from n will be C(n, 2)=n*(n-1)/2=O(n^2).

# Idea 2: 2 pointers going inwards

I will reveal the solution first and then analyze and prove it.
1. We need to use two pointers i and j. i starts from the 1st integer, j starts from the last integer.
2. Then, we calculate the area S(1, n). Here, S(i, j)=min(i, j)*(j-i).
(Important) What are the next two integers to form the container? We pick the shorter line between 1 and n, which is min(i, j). Let’s assume i=1 is shorter.
3. Then we move the pointer from i=1 to the next integer of i, which is i=2. At the same time, we hold j=n in position, which is the longer one. This time, we use i=2 and j=n to from a new container.
4. Calculate the area of the new container.
5. Always move the shorter one to the next integer (move towards the center: i moves right-wards, j moves left-wards) and hold the longer one in place to form the new container.
6. We also need a variable to record the max area among all possible S. It’s easy. We set up and initialize maxArea=-1. If we find any S>max, we assign the value of S to maxArea. Otherwise, we don’t change the value of maxArea so that maxArea always stands for the max value among S.

# Explanation 

1. This graph pictures the initialization stage: i < j. From the perspective of i=1, its best choice is j=n among all integers from j=2 to j=n. Why? Because the height of the container that i make up is at most i's length: if i=1 meets a longer one, the height will still be limited by i’s length; if i meets a shorter one, the height will be even worse. So, among all integers, i’s best strategy is to find the furthest integer, which is n.
2. If you understand the above, you understand 3 general properties for any pair(i, j) :
    * The shorter one doesn’t have to form containers with any other integers between i and j, since j beats all of them.
    * The shorter one may not be the longer one’s best choice.
    * The longer one still has the chance to form larger containers with lines in between i and j. So the longer one still needs to search.
3. If you understand the above, you are 90% to understand the solution.
    * In the solution, the pair(i, j) starts from (1, n). Let’s assume line i=1 is shorter. Then, we abandon i=1 and move the pointer i to the next integer in between i and j, which is i=2, since i=1 has already found its best choice but j=n hasn’t.
    * In the next pair (2, n), assume line j=n is shorter, we abandon j=n and move the pointer j to the next integer in between i and j, which is j=n-1, since j=n has found its best choice but i=2 hasn’t.
    * We repeat the above steps until two pointers meet. In this way, every integer has found its best choice.
4. Why T(n)=O(n)?
    * It's because in pair (1, n), we eliminate n-2 unnecessary comparisons for the shorter one.
    * In the second iteration (2, n) or (1, n-1), we eliminate n-3 unnecessary comparisons for the shorter one.
    ......
    * In the second last iteration, we eliminate 1 unnecessary comparison for the shorter one.
    * In the last iteration, we eliminate 0 unnecessary comparison for the shorter one.
    * In sum, we eliminate m=(n-2)+(n-3)+...+2+1 unnecessary comparisons from C(n, 2)=n*(n-1)/2. The actual comparisons that we implemented are C(n, 2)-m which is n-1.
5. Another way to interpret this T(n)=n-1 is that, we have implemented n-1 iterations. That means, we have calculated the area for n-1 containers. Why? Think about an easy scenario when n is the longest line among all. Then the iterations go from (1, n) to (n-1, n) because j=n never changes its place. In this scenario, we move i from 1 to n-1 so there are n-1 iterations in total.

**Reference**
https://leimao.github.io/blog/Proof-Container-With-Most-Water-Problem/
