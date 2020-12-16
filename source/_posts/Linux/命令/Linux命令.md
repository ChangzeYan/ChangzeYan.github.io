---
title: Linux命令
author: ChangzeYan
date: 2020-12-16 20:06:32
tags: 命令
categories: Linux
cover:
---

# 文件vi

## 命令模式
shift+:进入命令模式，在冒号后面写命令。
### 批量替换
文件内全部替换：
```bash
%s#abc#def#g（用def替换文件中所有的abc）
```

文件内局部替换：
把10行到50行内的“abc”全部替换成“def”
```bash
10,50s#abc#def#g（如文件内有#，可用/替换,:%s/abc/def/g）
```
以上命令如果在g后面再加上c，则会在替换之前显示提示符给用户确认（conform）是否需要替换。 比如：
```bash
:%s#linuxidc.com#linuxidc.net#gc
```
