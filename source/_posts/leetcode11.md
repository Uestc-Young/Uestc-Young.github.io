---
title: Leetcode 11
date: 2024-10-19 11:31:50
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

一开始想到的暴力`O(n^2)`的方法，遍历所有的组合，找到最大的面积。不过显然会TLE。
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        ans = 0
        for i in range(len(height)-1):
            for j in range(i+1, len(height)):
                bigger_height = min(height[i], height[j])
                area = bigger_height * (j - i)
                # print(area)
                if area <= ans:
                    continue
                else:
                    ans = area
        return ans
```

假设我们有两个指针，指向数组的两个位置，分别对应着高度 `x, y (x <= y)`，不妨假设他们之间的距离是 `t` 。那么这两个指针之间的面积就是 `min(x, y) * t = x * t` 。此时我们如果移动高的指针，得到新的 `y_1`。分两种情况讨论，如果 `y_1 < y`，那么新的面积就是 `min(x, y_1) * (t-1) <= min(x, y) * t`。如果 `y_1 >= y`，那么新的面积就是 `min(x, y_1) * (t-1) = x * (t-1)`。这样可以看出，如果我们移动高的指针，面积只会变小，所以我们应该移动矮的指针。

换句话说，其实可以类比木桶的“短板效应”，我们应该移动短的那一边，因为移动长的那一边，不管怎么移动，都不会使得面积变大。
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        i, j = 0, len(height)-1
        ans = (len(height)-1) * min(height[i], height[j])
        while i != j:
            if height[i] <= height[j]:
                i += 1
            else:
                j -= 1
            now_area = (j - i) * min(height[i], height[j])
            if now_area > ans:
                ans = now_area
        return ans
```