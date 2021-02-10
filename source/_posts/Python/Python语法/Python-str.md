---
title: Python-str
author: ChangzeYan
date: 2021-01-28 16:05:26
tags: String
categories: Python
cover:
---

# 字符串拼接

多个字符串拼接可以用join:
```python   
str_list = ['Hello', 'world']
str_join1 = ' '.join(str_list)
str_join2 = '-'.join(str_list)
print(str_join1) >>>Hello world
print(str_join2) >>>Hello-world
```

加号拼接:
```python
str_1 = 'Hello world！ '
str_2 = 'My name is Python猫.'
print(str_1 + str_2)
>>>Hello world！ My name is Python猫.
print(str_1)
>>>Hello world！
```