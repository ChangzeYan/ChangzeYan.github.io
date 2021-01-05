---
title: Hexo 命令
author: ChangzeYan
date: 2021-01-01 20:48:33
tags: hexo
categories: Npm
cover:
---

# 重建项目依赖
```shell
rm -rf node_modules && npm install --force
```

# 更新项目依赖
更新后会在package.json中看到变化，**但是可能会造成格式错误，提前做好package.json的备份**
```shell
 npm audit fix --force
```
