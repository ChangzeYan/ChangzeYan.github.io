---
title: Mongo语法
author: ChangzeYan
date: 2021-01-28 15:54:30
tags: Mongo
categories: Liunx
cover:
---

# 查询

## and
查询student 集合中名字为zs且id为02的项:
```bash
db.getCollection("student").find({"name":"zs","id":"02"})
```