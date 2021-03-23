---
title: Python-操作mongo
author: ChangzeYan
date: 2020-12-16 22:10:04
tags: Python-Mongo
categories: Python
cover:
---

# 安装依赖
```bash
pip install pymongo
```

# 获取连接

```py
from pymongo import MongoClient
# 无密码：
mongo_client = MongoClient(host='localhost', port=27017)

# 使用管理员的用户名和密码
myclient = pymongo.MongoClient('mongodb://root:123456@localhost:27017/')
```

使用数据库密码：
```py
mongo_client = MongoClient(host='ip', port=34843)
# 数据库名
db = mongo_client.my_mongodb
db.authenticate('username', 'xxxxx')
# collection名称
collection = db.student
# 打印前3条数据
for document in collection.find().limit(3):
    print(document)
```

# 插入
```python
json_dict = json.loads(json_str)  //将json字符串转成字典
mongo_collection.insert_one(json_dict)
```


# 加载mongo数据到pandas
```python
from pymongo import *
import json
import pandas as pd

mongo_client = MongoClient(host='xxx', port=xxx)
db = mongo_client.db_name
db.authenticate('username', 'pwd')
collection = db.student
data = pd.DataFrame(list(collection.find()))
print(data)
```