---
title: LeetCode 118.Pascal's Triangle
date: 2019-07-08 12:38:50
tags: LeetCode
keywords:
description:
---

# Question: [here](https://leetcode.com/problems/pascals-trianglw/)
Given a non-negative integer **numRows**, generate the first **numRows** of Pascal's triangle.
![]()
## Example:

Input: [1,null,2,3]
1
\
2
/
3
Output: [1,2,3]
这一题用递归很简单，遍历就行，但因为遍历有两个常用的方法：一是递归(recursive)，二是使用栈实现的迭代版本(stack+iterative)。这两种方法都是O(n)的空间复杂度（递归本身占用stack空间或者用户自定义的stack），那但如何使空间复杂度为O（1）呢？

<!-- more -->
