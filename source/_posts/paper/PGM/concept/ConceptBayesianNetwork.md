---
title: 贝叶斯
date: 2019-06-15 14:45:32
author: ChangzeYan
tags:
  - paper
  - PGM
  - BN
categories: paper
cover: false
mathjax: True
---

## 贝叶斯

### 朴素贝叶斯分类

#### 贝叶斯定理
已知某条件下的概率，如何得到两条件交换后的概率，也就是在已知 $P(A|B)$ 的情况下如何求得 $P(B|A)$ 的概率。 **$P(A|B)$ 是后验概率（posterior probability），也就是我们常说的条件概率**。即在条件 $B$ 下，事件 $A$ 发生的概率。**相反 $P(A)$ 或 $P(B)$ 称为先验概率（prior probability）**。
适用于当很容易直接得出 $P(A|B)$，$P(B|A)$ 则很难直接得出，可由 $P(A|B)$ 计算得到 $P(B|A)$
$$
P(B|A)=\frac{P(A|B)P(B)}{P(A)}
$$

### 贝叶斯网
`Bayesian Network`，采用有向无环图表示网络结构，使用条件概率表`CPT`来描述联合概率分布。

### 参考
[概率图模型之：贝叶斯网络](https://blog.csdn.net/gnahznib/article/details/70244175)
