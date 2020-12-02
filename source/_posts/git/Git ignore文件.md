---
title: Git ignore 文件
author: ChangzeYan
date: 2020-11-30 11:14:45
tags:
categories:
  - Git
cover:
---

# .gitignore
参考：[git设置忽略文件和目录](https://www.cnblogs.com/wangmo/p/7737109.html)
## 忽略正则
忽略*.o文件和 *.a文件：
```bash
*.[oa]
```

忽略*.b 和 *.B文件，my.b除外
```bash
*.[bB]
!my.b
```
忽略dbg文件和dbg目录
```bash
dbg
```

只忽略dbg目录，不忽略dbg文件

```bash
dbg/
```
.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。

# 删除github中某个文件夹
参考：[删除文件夹](https://blog.csdn.net/wudinaniya/article/details/77508229)
在github上只能删除文件，删除文件夹的方式如下：

在本地仓库：
```bash
git rm -r --cached target         # 删除target文件夹
git commit -m '删除了target文件夹'      # 提交,添加操作说明
git push -u origin master        # 将本次更改更新到github项目上去
```
