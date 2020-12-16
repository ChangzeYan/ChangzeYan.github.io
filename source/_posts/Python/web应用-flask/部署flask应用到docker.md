---
title: 部署flask应用到docker
author: ChangzeYan
date: 2020-12-16 13:04:27
tags: Flask
categories: Python
cover:
---

# 导出与项目有关的依赖
## 安装pipreqs
```bash
pip install pipreqs
```
## 使用
进入到项目根目录，如果用conda装的python，需要切换到项目的python环境：
```bash
activate python36
```
然后执行：
```bash
pipreqs ./
```
成功后在项目根目录下生成requirements.txt文件。

如果报下面错误：

![安装pipreqs](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/flask部署到docker-安装pipreqs.png)
按照路径提示，修改pipreqs.py文件：
将第74行中的
```py
encoding=encoding
改为：
encoding='utf-8'
```
再次执行 pipreqs ./

新环境下安装
```bash
pip install -r requirements.txt
```
