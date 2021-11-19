---
title: Excel 
author: ChangzeYan
date: 2021-09-16 18:58:22
tags: 
categories: Excel
cover:
description: EXCEL常用函数
---

# 函数

## 求和函数
两行对应的数值相乘求和：
B2列到AB2列这一行与B3:AB3列这一行对应相乘求和
```
=SUMPRODUCT($B$2:$AB$2, B3:AB3)
```