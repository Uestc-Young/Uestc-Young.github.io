---
title: Leetcode 3
date: 2024-10-21 20:46:56
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---

给定一个字符串 `s`，请你找出其中不含有重复字符的 **最长子串** 的长度。

写个时间复杂度是 `O(n^2)` 的暴力，过了但是击败了 `5%` 的用户。
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        n = len(s)
        if n == 0:
            return 0
        ans = 1
        for i in range(n):
            now_length = 1
            seen = set()
            seen.add(s[i])
            for j in range(i+1, n):
                if s[j] not in seen:
                    seen.add(s[j])
                    now_length += 1
                    ans = max(now_length, ans)
                else:
                    break
        return ans
```