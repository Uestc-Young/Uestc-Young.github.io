---
title: leetcode128
date: 2024-10-17 16:39:49
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---

给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

题目的意思是要找出一个连续的序列，不要求在原数组中连续，只要求数字连续即可。同时对于这种数据: `[0, 0, 1, 2, 3, 4]` ，其实只需要找到 `[0, 1, 2, 3, 4]` 即可，所以可以先去重 `set()` ，然后排序 `sorted()` ，然后遍历一遍即可。

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if nums == []:
            return 0
        new_list = sorted(set(nums))
        ans = 1
        longest = 1
        # print(new_list)
        for i in range(0, len(new_list)-1):
            if new_list[i+1] == new_list[i] + 1:
                ans += 1
                if ans >= longest:
                    longest = ans
                continue
            ans = 1
        return longest
```