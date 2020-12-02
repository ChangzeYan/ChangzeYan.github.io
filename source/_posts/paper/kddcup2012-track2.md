---
title: KDD-Cup2012-Track2
author: ChangzeYan
date: 2019-06-20 14:02:44
tags:
  - paper
  - KDD
categories: Paper
cover: false
mathjax: True
---


## [KDD-Cup2012-Track2](https://jyunyu.csie.org/docs/pubs/kddcup2012paper.pdf)

### A Two-Stage Ensemble of Diverse Models for Advertisement Ranking in KDD Cup 2012

### 问题描述
一次会话是指用户和搜索引擎的互动，即一次查询操作。会话包括：用户、搜索内容、搜索引擎搜到的广告、搜索引擎呈现给用户的广告、用户点击的广告（0条或者多条）、搜索会话的深度是指：搜索引擎呈现给用户的广告的数量；广告的位置/position（排名、rank）是指该广告在呈现给用户所有广告中的排名，呈现给用户的所有广告都会生成一个短文本，即广告的标题，标题后面紧跟着一个长文本（即：广告的描述）和一个URL（URL通常被缩短以节省屏幕空间）。

### 训练集
测试集中的每行数据都有12个，含义依次如下：
1. `Click`: 用户（`UserID`）点击广告（`AdID`）的次数
2. `Impression`: 广告(`AdID`)呈现给用户（`UserID`）的次数(每次查询只会呈现一次)
3. `DisplayURL`: URL总是随着标题和描述出现，在文件中，URL是匿名的哈希值
4. `AdID`: 广告id
5. `AdvertiserID`: 广告商id
6. `Depth`: 一次会话呈现给用户的广告数量
7. `Position`: 广告在返回给用户广告列表的index
8. `QueryID`: 搜索id，从0开始的整数，是queryid_tokensid.txt的主键
9. `KeywordID`: 关键词id，buyingkeyword_tokensid.txt的主键
10. `TitleID`: titleid_tokensid.txt的主键
11. `DescriptionID`: descriptionid_tokensid.txt的主键
12. `UserID`: userid_profile.txt的主键，当无法识别用户时，此字段的值为0。


>注：要预测的是 $\frac{Click}{Impression}$ ，后面二分类的时候，把`Click`当成正样本集，`Impression-Click` 当作负样本集

### 其他附属文件
1. queryid_tokensid.txt
2. buyingkeywordid_tokensid.txt
3. titleid_tokensid.txt
4. descriptionid_tokensid.txt
5. userid_profile.txt

>对于前4个文件，每一行都是 `id query|keyword|ad title|ad description`的格式,这些数据可能是自然语言，在文件中都用其哈希匿名化。
>'userid_profile.txt'的每一行由UserID，Gender和Age组成，由TAB字符分隔。注：并非训练和测试集中的每个UserID都将出现在'userid_profile.txt'中。 每个字段描述如下：

```yaml
Gender: '1' (男) '2'(女) '0'(unknow)
Age:  '1' for (0, 12],  '2' for (12, 18], '3' for (18, 24], '4'  for  (24, 30], '5' for (30,  40], and '6' for greater than 40.
```
### 测试集
除了广告的被呈现次数(Impression)和点击次数(Click)，测试集和训练集保持相同的格式，测试数据集的一个子集用于在排行榜上对提交/更新的结果进行排名

### 需要提交的结果
一个文本文件，每一行只有一个字段：广告的点击率，并且按照顺序与所给文件的行一一对应。

[标注版论文](/assets/kddcup2012paper_note.pdf)

### 参考
[1]杨之之.Kaggle[2] - Predict the click through rate (KDD12 trackl2)[EB/OL].https://blog.csdn.net/u011292007/article/details/36886523 2014-07-09/2019-06-20
[2]https://www.kaggle.com/c/kddcup2012-track2/overview
