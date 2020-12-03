---
layout: w
title: Git命令
date: 2020-12-03 10:51:18
tags:
categories:
  - Git
author: ChangzeYan
cover:
---

# 查看
```bash
# 查看添加到git的文件
git ls-files
```

## 添加文件不成功
git add出现 "fatal: in unpopulated submodule XXX" 错误
在本地仓库如果克隆了其他远程仓库，该远程仓库就会git add失败，解决：
```bash
git rm -rf --cached xxxx(文件夹的名称)
git add xxxx/*
```
