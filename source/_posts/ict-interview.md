---
title: 中国科学院计算技术研究所客座学生面试
date: 2024-10-07 22:50:27
tags:
- [Life]
- [面试]
mathjax: true
typora-root-url: ..
---

## 中国科学院计算技术研究所客座学生 一面(40min)

面试是计算所的phd电话交流的，流程如下：

- 介绍了要做的项目

    ICT，商汤和南方电网一起合作的项目。主要做LLM+时序预测，有卡有数据集，但是数据集是南方电网的内部私有数据集，不能公开。
-   谈谈之前做过的LLM相关的项目，有没有自己跑过，或者测试过一些LLM？

    讲了远程实习那段用LLM做漏洞检测和漏洞修复的工作。

<!--more-->

- pytorch熟不熟？深度学习代码实现能力怎么样？

    讲了自己比较熟悉NLP相关的，CV几乎没怎么做过。

- 为什么要分成NLP和CV两个领域，pytorch代码不应该是差不多的吗？

    讲了自己在澳大那边做自动驾驶的时候，遇见的PyG这个包里面会有用来处理图节点之间信息传递的函数，要看一下官方文档才知道到底是用来做什么的。


- 那讲一下BERT，对BERT熟悉吗，它的训练任务是什么，它和GPT之间有什么区别吗？

    BERT:
    用过，做过一个情感分析的项目。架构是用的transformer encoder，双向。训练任务分为两种，一种是mask掉一部分词语，让模型预测这些词语是什么，另一种是给定两个句子，让模型判断这两个句子是否是连续的。用的损失函数是交叉熵，优化器是Adam。具体训练任务是用的CLS token的输出来做分类任务。BERT主要用在NLU(Natural Language Understanding)任务上，比如文本分类，情感分析等。
    
    GPT:
    没接触过，架构用的是transformer decoder，训练任务就是词语接龙。损失函数会最大化下一个词的概率。用的是自回归的方式，每次预测下一个词的时候，会把前面的词作为输入。优化器是Adam。GPT主要用在NLG(Natural Language Generation)任务上，比如文本生成，对话生成等。

- 用过tensorRT吗，分布式了解过吗，多卡训练有经验吗？需要用tensorRT测一些模型的latency，属于一些杂活，文章会挂名能不能接受？
  
    完全没用过~~但是可以学~~。多卡只跑过inference。完全没问题。

- 数学怎么样？二项分布，伯努利分布，正态分布这三者之间有什么关系？大数定律知道吗？做个场景题：[优惠券问题](https://zhuanlan.zhihu.com/p/140537355)。

    伯努利实验就是0-1分布，二项分布是n次伯努利实验，正态分布是二项分布的极限分布。大数定律是指随着样本数量的增加，样本均值会逐渐收敛到总体均值。

    场景题完全不会）

- 来问点传统的算法，讲讲快速排序是怎么样的，时间复杂度是多少？还有哪些排序算法可以说一说的？
    
    快速排序具体做法是先选一个基准值，然后把比基准值小的放在左边，比基准值大的放在右边，然后递归的对左右两边进行排序。时间复杂度是O(nlogn)。其他排序讲了堆排，具体做法是先构建一个大顶堆，然后把堆顶元素和最后一个元素交换，然后把剩下的元素重新构建大顶堆，重复这个过程直到所有元素都排好序。时间复杂度是O(nlogn)。

- 有什么问题要问的吗？

    随便问了一点关于项目的问题。

update: oc，已拒绝。