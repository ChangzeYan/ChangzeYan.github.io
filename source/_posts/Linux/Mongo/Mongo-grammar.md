---
title: Mongo语法
author: ChangzeYan
date: 2021-01-28 15:54:30
tags: Mongo
categories: Liunx
cover:
---

# 查询

## 查询所有
```bash
db.collection.find()
```

## 获取数据条数
```bash
db.collection.find().count()
```

## 范围查询
```bash
查询age>22的记录
db.userInfo.find((age:{$gt:22}});
查询age<22的记录
db.userInfo.find({age:{$lt:22}});
查询age>=25的记录
db.userInfo.find({age:{$gte:25}});
查询age<=25的记录
db.userInfo.find({age:{$lte:25}});
查询age>23并且age<=26
db.userInfo.find({age:{gte:23,$lte:26}});
```

## and
查询student 集合中名字为zs且id为02的项:
```bash
db.getCollection("student").find({"name":"zs","id":"02"})
```
