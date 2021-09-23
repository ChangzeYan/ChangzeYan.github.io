---
title: Conda 命令
author: ChangzeYan
date: 2020-12-15 13:24:09
tags: Conda
categories: Python
cover:
---

# 安装包
## 指定下载源
pip install xxx Could not fetch URL https://pypi.tuna.tsinghua.edu.cn/simple/pipenv/
解决，常用的镜像地址有：
```bash
# 阿里云
http://mirrors.aliyun.com/pypi/simple/
# 中国科技大学
https://pypi.mirrors.ustc.edu.cn/simple/
# 豆瓣
http://pypi.douban.com/simple/
# 清华大学
https://pypi.tuna.tsinghua.edu.cn/simple/
# 中国科学技术大学
http://pypi.mirrors.ustc.edu.cn/simple/
```

安装时指定下载源：
```bash
pip install xxx -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```

## 指定安装包的版本

```py
# 安装
conda install numpy=1.9.3
# 更新
conda update numpy=1.9.3
```

# 查看虚拟环境
```bash
conda info --envs
```



创建python环境:

先在conda中创建一个名为python2的环境，并下载对应版本python2.7
```bash
conda create --name python27 python=2.7

或
conda create -n python36 python=3.6.6
conda create -n yourenvname python=x.x anaconda (还会创建与python版本有关的anaconda打包库)
```

激活（切换）python环境
```
activate yourenvname
```
退出虚拟环境：
deactivate myenv 

删除虚拟环境

```
conda remove -n yourenvname --all
```