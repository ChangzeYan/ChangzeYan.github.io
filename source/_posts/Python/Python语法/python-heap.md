---
title: Python-堆
author: ChangzeYan
date: 2021-01-26 12:05:51
tags: Heap
categories: Python
cover:
---

# 使用
```python
from heapq import * 
# 建立堆
heap=[]

# 将x压入堆中
heappush(heap, x)    
# 从堆中弹出最小的元素              
heappop(heap)   

# 让列表具备堆特征，函数heapify通过执行尽可能少的移位操作将列表变成合法的堆（即具备堆特征）。如果你的堆并不是使用heappush创建的，应在使用heappush和heappop之前使用这个函数。
heapify(heap) 
# 弹出最小的元素，并将x压入堆中
heapreplace(heap, x)        

# 返回iter中n个最大的元素
nlargest(n, iter)                           
# 返回iter中n个最小的元素            
nsmallest(n, iter)                                   
```

# 自定义排序
```python
class Document:
    def __init__(self, _id, con, goal):
        self.id = str(_id)
        self.content = con
        self.goal = goal

    # 按照goal，从小到大排序
    def __lt__(self, other):
        if self.goal < other.goal:
            return True
        else:
            return False

d1=Document('1',"fda",23)
d2=Document('2',"fda",2)
d3=Document('3',"fda",245)

li=[d1,d2,d3]
li.sort()
```

