---
title: Linux安装Python3.6
author: ChangzeYan
date: 2021-01-27 19:26:20
tags: Install
categories: Linux
cover:
---
参考: [Centos7安装python3.6.5
](https://www.cnblogs.com/yangzhaon/p/11203395.html)

安装python3.6.5,原来的python2.7并存

# 安装依赖
```bash
yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel wget
```

下载安装包:
```bash
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
```

解压文件:
```bash
tar -zxvf Python-3.6.5.tgz
```
进入 解压后的目录,指定编译后文件的目录:
```bash
cd Python-3.6.5.tgz

./configure --prefix=/usr/local/python3
```

编译:
```bash
make && make install
```

如果出现zipimport.ZipImportError: can't decompress data; zlib not avaliable,则安装:
```bash
yum install -y zlib*
```
然后重新执行make

# 设置软链
查看 /usr/bin下是否有pip3和python3,如果没有设置下面的软链:
```bash
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```


