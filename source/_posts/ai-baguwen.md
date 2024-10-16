---
title: AI面试八股文
date: 2024-10-15 22:28:04
tags:
- [Life]
- [面试]
mathjax: true
typora-root-url: ..
---

## Softmax为什么要减去最大值
对于一组，例如[1000, 1000, 1000]的数据，如果直接计算softmax，会导致数值溢出，所以我们需要减去最大值，使得数值更稳定。而且从数学上来说，减去最大值不会改变softmax的结果。

## Attention里面为什么要除以$\sqrt{d_k}$
除以$\sqrt{d_k}$是为了使得输入和输出的方差相近，这样在softmax之前的输出不会过大，导致梯度消失或者爆炸。 

