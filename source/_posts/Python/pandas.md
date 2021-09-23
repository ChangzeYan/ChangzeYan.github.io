---
title: Pandas
author: ChangzeYan
date: 2021-03-13 19:51:15
tags: pandas
categories: Python
cover:
---

# DataFrame
创建dataframe
```
df = pd.DataFrame(columns=['a','b'])
```
给dataframe增加行：
```
for i in range(5):
    df.loc[i]=[1,2,3]
```
## 排序
对行排序：
```
按照'阅读'列将行降序排列
sort_res=df.sort_index(axis=0, by='阅读', ascending=False)
```
## 保存
保存到excel中：
```
df.to_excel('test.xls')
```

# 统计某列值出现的次数
```python
import pandas as pd
import numpy as np
df = pd.DataFrame({'key1':['a','a','b','b','a'],'key2':['one','two','one','two','one'],'data1':np.random.randn(5),'data2':np.random.randn(5)})
```
![dataframe](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/pandas_datadf.png)

统计key2中各个元素的出现次数：
```python
df['key2'].value_counts()  //返回series
```
![key2中各个元素的出现次数](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/df_value_count.png)


# Series

## 遍历series
```python
series = data['author'].value_counts()
for i, v in series.items():
    print(i, v)
```
