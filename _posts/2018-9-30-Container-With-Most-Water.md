---
layout:     post
title:      "LeetCode 11. Container With Most Water"
subtitle:   "Array, Two Pointers"
date:       2018-9-30 12:00:00
author:     "baiyf"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.3
catalog:    true
tags:
    - LeetCode
---

# 11. Container With Most Water

Given *n* non-negative integers *a1*, *a2*, ..., *an* , where each represents a point at coordinate (*i*, *ai*). *n* vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and *n* is at least 2.

 

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

 

**Example:**

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

## 题意
有一个非负的整数数组，寻找这个数组内能形成的一个容量最大的区域
这道题可以用到双指针的方法，分别为`left`和`right`指针

最大容量为`min(left_height, right_height) * (right - left)`， 同时将值较小一边的指针向前（后）递推，继续比较。
整个时间复杂度是`O（n）`

## 解法
```python
class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        length = len(height)
        left, right = 0, length-1
        ans = 0
        while left < right:
            if height[left] <= height[right]:
                contain = (right - left) * height[left]
                left += 1
            else:
                contain = (right - left) * height[right]
                right -= 1
            if contain > ans:
                ans = contain
        return ans
```
