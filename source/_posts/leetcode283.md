---
title: Leetcode 283
date: 2024-10-19 10:21:49
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

请注意 ，必须在不复制数组的情况下原地对数组进行操作。

想到的第一种方法用的是python的 `remove` 函数，但是这个函数的时间复杂度是 $O(n)$，所以这个方法的时间复杂度是 $O(n^2)$，不是很好。
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        count = 0
        i = 0
        while i < len(nums):
            if nums[i] == 0:
                nums.remove(0)
                count += 1
            else:
                i += 1
        for _ in range(0, count):
            nums.append(0)
```

还可以用双指针来优化一下结果，只需要遍历一次数组，时间复杂度是 $O(n)$。主体思想是用一个指针 `j` 来记录非零元素的位置，然后遍历数组，如果当前元素不是0，就把当前元素赋值给 `nums[j]`，然后 `j` 自增1，最后再把 `j` 后面的元素全部赋值为0。
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        j = 0
        count = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[j] = nums[i]
                j += 1
            else:
                count += 1 
        for k in range(0, count):
            nums[j+k] = 0
```