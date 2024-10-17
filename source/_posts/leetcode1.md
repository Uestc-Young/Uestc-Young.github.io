---
title: Leetcode 1
date: 2024-10-16 20:01:31
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---

两数之和

最暴力的想法就是直接两层循环，遍历所有的可能性，找到符合条件的两个数即可。时间复杂度是$O(n^2)$。
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i] + nums[j] == target:
                    return i, j
```

这里可以用hash表来优化，时间复杂度是$O(n)$。用元素的值作为key，元素的下标作为value，这样就可以通过查找target-num是否在hash表中来判断是否存在这样的两个数。
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        store = {}
        for i, num in enumerate(nums):
            if target - num in store:
                return [store[target-num], i]
            store[num] = i
        return []
```