## DP动态规划的一些想法 + Leetcode120 Solution

**dp的本质是：当前的分析要建立在过往分析的基础上。所以过往分析一定要做到两点，一个是全，一个是优。**

对于全：如果对过往分析的成果保存不全（比如：用新数据覆盖了，或者根本没存）那么信息的缺失一定会导致结果的局限性，且不能达到最优。

对于优：即存下来的东西一定要是最优的结果。但是很多人在找动态转移方程时就会犯一个错误，为了优而放弃了全，用最优的新数据覆盖了后面可能还会需要用到的数据。在我看来这是DP用不好的重要原因之一。

所以为了兼顾优和全，储存DP结果的这个list的一定要构造好。把需要做到全的结果，当成需要存储的一个维度，这样一般就可以解决上面这个问题。



不如让我举个🌰：

Leetcode 120

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is `11` (i.e., **2** + **3** + **5** + **1** = 11).

**Note:**

Bonus point if you are able to do this using only *O*(*n*) extra space, where *n* is the total number of rows in the triangle.

1. DP

   一看就是用动态规划，但是如何构造DP List呢？即每个$dp[i][j]$应该存点什么。肯定是最短路径的值，但是是什么到什么的最短路径呢？我一开始的想法就犯了上面的错误，我考虑的是用一维array，每个$dp[i]$存储每层最短路径的值，$dp[0] = 2，dp[1] = 5，dp[2] = 10，dp[3] = 11$这样肯定是不对的，每个点的信息并没有存储全，从而也无法根据过往的信息作出最优的选择。

   然后我又想到了从下往上走，但是使用一维array还是不够存储所有点的信息，因为只有遍历完所有点，存储每一个点的信息，才可以做出最优决策。所以这个array一定是二维的。

   既然是二维的，那么$dp[i][j]$里每个i, j应该存些什么？考虑到dp的从小到大策略，再加上上面刚刚的想法，那么很容易就想到一个维度i代表每一层，另一个维度j代表一层的某一点，然后仍然可以借鉴刚刚从下往上走的想法。这样代码就出来了。

```python
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        n = len(triangle)
        dp = [[0]*n for _ in range(n-1)]
        dp.append(triangle[-1])
        for i in range(n-2, -1, -1):
            for j in range(0, i+1):
                dp[i][j] = min(triangle[i][j] + dp[i+1][j], triangle[i][j] + dp[i+1][j+1])

        return dp[0][0]
```



**同样的，有时候保留过多的信息，反而增加了空间复杂度。就可以做一些适当的优化。**

比如，这一题

213. House Robber II

Medium

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example 1:**

```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```

**Example 2:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

思路是动态规划，以及如何处理头尾部分。解决头位问题的办法就是跑两遍for循环，一遍一定偷头不偷尾，一遍一定偷尾不偷头。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums: return 0
        if len(nums) <= 3: return max(nums)
        dp1 = [0] * len(nums)
        dp2 = [0] * len(nums)
        for i in range(len(nums)-1):
            dp1[i] = max(nums[i]+dp1[i-2], dp1[i-1])
            
        for i in range(1, len(nums)):
            dp2[i] = max(nums[i]+dp2[i-2], dp2[i-1])
        
        
        return max(dp1[-2], dp2[-1])
```

改良前的dp算法使用两个dp list进行存储。但是我们也可以发现，对于每一个dp list，我们只用到了他最近的两个数字$dp[i-1],dp[i-2]$，所以完全可以不用list，而是两个variable就够了。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums: return 0
        if len(nums) <= 3: return max(nums)
        cur = 0 # dp[i-1]
        pre = 0 # dp[i-2]
        for i in range(len(nums)-1):
            cur, pre = max(nums[i]+pre, cur), cur
        temp = cur
            
        cur = 0 # dp[i-1]
        pre = 0 # dp[i-2]
        for i in range(1, len(nums)):
            cur, pre = max(nums[i]+pre, cur), cur
        
        return max(temp, cur)
```

⚠️以下两种写法并不等价

```python
pre = cur
cur = max(nums[i]+pre, cur)
```

```python
cur, pre = max(nums[i]+pre, cur), cur
```



**同时，我们还要去减少一些无用的搜索。比如在寻找最优解的时候，可能会出现从头遍历一遍找到最优解，那不如直接每一步就存储好（或者就更新到）最优解，直接下一步就可以拿来用，这样就减少了搜索时间，同时也能减少存储时间。**这样的过程一般适用于将原本的二维dp 矩阵简化到一维。这种简化不建议一次性实现，或者一看到题就想如何用一维dp array实现，因为：1. 一维到底是选取二维中的哪一个个维度？这一般只有实现过一次二维才会有想法。2. 遍历的顺序？哪怕只有一个维度，还是需要for 循环两遍的，那是先循环维度一还是维度二也是一个问题，其本质还是选取一维维度的问题。

**总之，动态规划问题应该是一个由全到简的过程，想要一步登天（用一维矩阵又快又好的实现）除非熟练度上来，不然不建议。**