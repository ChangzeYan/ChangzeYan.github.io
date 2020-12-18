---
title: Python-操作mongo
author: ChangzeYan
date: 2020-12-16 22:10:04
tags: 操作Mongo
categories: Python
cover:
---

# 安装依赖
```bash
pip install pymongo
```

# 获取连接

```py
# 无密码：
mongo_client = MongoClient(host='localhost', port=27017)

# 使用管理员的用户名和密码
myclient = pymongo.MongoClient('mongodb://root:123456@localhost:27017/')
```

使用数据库密码：
```py
mongo_client = MongoClient(host='ip', port=34843)
db = mongo_client.my_mongodb
db.authenticate('username', 'xxxxx')
collection = db.student
```
