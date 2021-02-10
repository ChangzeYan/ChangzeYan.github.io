---
title: Pip
author: ChangzeYan
date: 2021-02-10 10:32:04
tags: Pip
categories: Python
cover:
---

## 查看pip 安装了那些包
```bash
查看所有
pip list

查看是否安装了某个包
centos
pip list|grep pandas

windows
pip list | findstr pandas
```
## 查看
```bash
4.查看已安装过的包
pip show - -files Somepackage
5.检查哪些包需要更新
pip list - -outdated
```

## 升级
```bash
6.升级包
pip install --upgrade Somepackage

升级pip：
python -m pip install --upgrade pip
或者：pip install -U pip
```