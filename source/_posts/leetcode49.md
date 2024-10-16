---
title: Leetcode 49
date: 2024-10-16 20:17:04
tags: [Algorithm]
mathjax: true
typora-root-url: ..
---

给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
字母异位词 是由重新排列源单词的所有字母得到的一个新单词。

Python里面的`sorted`函数可以直接对字符串排序，最后返回的是列表，可以用`''.join()`来转换成字符串。

我的解法时间复杂度是$O(n)$。
```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        seen = {}
        for i, word in enumerate(strs):
            sorted_word = ''.join(sorted(word))
            if sorted_word not in seen:
                seen[sorted_word] = [word]
            else:   
                seen[sorted_word].append(word)     
        # for key, value in seen.items():
        #     ans.append(value)
        return list(seen.values())
```
力扣官网给的是用了`collections.defaultdict`的方法，这样就不用判断key是否在字典里面了。