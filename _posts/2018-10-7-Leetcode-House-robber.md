---
layout:     post
title:      "LeetCode 198&213&337. House Robber I&II&III"
subtitle:   "Dynamic Programming"
date:       2018-10-7 12:00:00
author:     "baiyf"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.3
catalog:    true
tags:
    - LeetCode
---

# 198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example 1:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

## 题意

这道题是一道经典的动态规划题目，解题的基本思想是从子问题最优解开始求解，即只有两个house时的求解问题

在最优子问题上的递推公式是$$dp[i] = max(dp[i-1], dp[i-2] + nums[i])$$

## 解法

```python
def rob(nums):
    if len(nums) == 0:
        return 0
    i = 0
    ans = [0] * len(nums)
    while i < len(nums):
        if i == 0:
            ans[i] = nums[i]
        elif i == 1:
            ans[i] = max(nums[i],nums[i-1])
        else:
            ans[i] = max(ans[i-1],ans[i-2] + nums[i]) #判断nums[i]是否能带来更优解
        i = i + 1
    return ans[-1]
```



# 213. House Robber II

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

## 题意

这道题目与上一道基本一样，区别是加入了一个限制条件，假设这个list是一个首尾相接的环，所以不能即抢头又抢尾

这道题还可以用上一题的方法，“掐头去尾”跑两次，即将nums去掉第一个数dp跑一次得到`left`，将nums去掉最后一个数dp跑一次得到`right`，最后返回`max(left, right)`

## 解法

```python
class Solution:
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 1:
            return nums[0]
        def one_turn(nums):
            if len(nums) == 0:
                return 0
            i = 0
            ans = [0] * len(nums)
            while i < len(nums):
                if i == 0:
                    ans[i] = nums[i]
                elif i == 1:
                    ans[i] = max(nums[i],nums[i-1])
                else:
                    ans[i] = max(ans[i-1],ans[i-2] + nums[i])
                i = i + 1
            return ans[-1]
        
        return max(one_turn(nums[:-1]), one_turn(nums[1:]))
```

# 337. House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

**Example 2:**

```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

## 题意

相比于前面两道题这道的变化是，假设house是二叉树的结构，小偷不能偷相邻的两个house，求能偷到的最大金额，方法依然是dp

因为是二叉树结构所以可以用递归快速的解决这个问题, 每一个节点保存两个数(norob, robCur), 分别表示dp[i-1]和dp[i]

递推公式是$$dp[i] = max(dp[i-2]+val(root), dp[i-1])$$

## 解法

```python
class Solution(object):
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        def dfs(root) :
            if not root : return [0, 0]
            robLeft = dfs(root.left)
            robRight = dfs(root.right)
            norobCur = robLeft[1] + robRight[1]
            robCur = max(robLeft[0] + robRight[0] + root.val, norobCur)
            return [norobCur, robCur]
        return dfs(root)[1]
```

