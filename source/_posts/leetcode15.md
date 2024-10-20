---
title: Leetcode 15
date: 2024-10-20 10:54:45
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---
给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j、i != k 且 j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

首先，最暴力的方法就是 `O(n^3)` 的时间复杂度，直接三重循环，找到所有的三元组，然后再去重。

但是在固定一个数之后，本质上又转化成了两数之和的问题，所以可以用双指针来优化一下。

这是一个复杂度为 `O(n^2)` 的算法，通过了 `308/313` 的测试用例。
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        ans = []
        for i in range(len(nums)):
            target = -nums[i]
            seen = {}
            for j in range(i+1, len(nums)):
                target_list = sorted([-target, nums[j], target-nums[j]])
                if target - nums[j] in seen and target_list not in ans:
                    ans.append(target_list)
                seen[nums[j]] = j        
        return ans
```

上面的算法在中间的去重操作上还需要遍历一次 `ans` 数组，所以可以用一个 `set` 来代替 `ans` 数组，这样就不需要再去重了。而且在python中，`set` 的查找操作 `in` 是 `O(1)` 的时间复杂度，所以这样的优化是可行的。

这种算法时间复杂度是 `O(n^2)`，通过了 `312/313` 的测试用例。
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        ans = set()  # 用集合去重
        for i in range(len(nums)):
            target = -nums[i]
            seen = {}
            for j in range(i + 1, len(nums)):
                if target - nums[j] in seen:
                    triplet = tuple(sorted([-target, nums[j], target - nums[j]]))
                    ans.add(triplet)  # 直接将结果加入集合
                seen[nums[j]] = j
        return [list(triplet) for triplet in ans]  # 将最终结果转为列表返回
```

看起来暴力是不行的，那么就换一种思路。对于原数组我们在一开始就进行排序，然后固定一个数，再用双指针来找另外两个数。在寻找另外两个数的时候，如果两个数的和大于目标值，那么右指针左移；如果两个数的和小于目标值，那么左指针右移；如果两个数的和等于目标值，那么就找到了一个三元组。这种算法的时间复杂度是 `O(n^2)`。同时在这里，固定第一个元素的时候，如果和上一个元素相同，那么就跳过这个元素，因为这个元素已经找过了。

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        sorted_list = sorted(nums)
        ans = set()
        for i in range(len(sorted_list)):
            if i > 0 and sorted_list[i] == sorted_list[i-1]:
                continue
            target = -sorted_list[i]
            left = i+1
            right = len(sorted_list)-1
            while left < right:
                if sorted_list[left] + sorted_list[right] > target:
                    right -= 1
                    continue
                if sorted_list[left] + sorted_list[right] < target:
                    left += 1
                    continue
                if sorted_list[left] + sorted_list[right] == target:
                    triplet = tuple(sorted([-target, sorted_list[left], sorted_list[right]]))
                    ans.add(triplet)
                    right -= 1
                    continue
        return [list(triplet) for triplet in ans]
```