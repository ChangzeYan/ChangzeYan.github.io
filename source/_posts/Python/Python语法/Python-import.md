---
title: Python-import
author: ChangzeYan
date: 2020-12-17 21:45:46
tags: 语法
categories: Python
cover:
---

# 同一包内import
问题分两种情况
1. 如果你是自己手动建的文件夹，你可以直接import
2. 如果你是用pycharm的新建python package，你新建的目录下就会有一个__init__.py文件


Trie.py文件与__init__.py文件在同一目录NER下，在__init__.py中importTrie.py：
```py
'''__init__.py '''
from NER.Trie import Trie_Ope
```

如果仍不能导入，在导包的前面加入这样一段代码:
```py
import os, sys
current_dir = os.path.abspath(os.path.dirname(__file__))
sys.path.append(current_dir)
```

这样导入在部署的时候可能出错，找不到 module NER，解决：将__init__.py文件改成其他的名称，这时这个文件夹就不是python package了，然后右键文件夹选择 mark as Sources root，然后导入：
```python
from Trie import Trie_Ope
```