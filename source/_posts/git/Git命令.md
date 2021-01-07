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

# 关联远程仓库并上传
```bash
git init
将本地项目工作区的所有文件添加到暂存区
git add .

# 可选
{
    git config user.email "2155513297@qq.com"
    git config user.name "Winds-Y"
}    

git commit -m "firstCommit"  (若有修改或添加文件执行)
关联远程仓库：origin 为远程仓库名
git remote add origin  https://github.com/Yahtz/TryBmob.git（远程仓库地址）
# 多用户：
{
    # git remote add origin git@ChangzeYan:ChangzeYan/test.git
    # git remote add origin git@Winds-Y:Winds-Y/test.git
}
git push -u origin master 
（若远程仓库有readme.md等本地仓库没有的文件，需要先pull）
（git pull origin master --allow-unrelated-histories）
或者
（git pull --rebase origin master）
然后再push
```

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
