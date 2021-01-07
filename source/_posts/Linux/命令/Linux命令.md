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
## 查找
```bash
/输入要搜索的字符串或者字符，然后回车 （从前往后找） 
按下n找下一个匹配项
按下N找上一个匹配项
?要搜索的字符串或者字符 （从后往前找）
命令模式下输入:noh 退出查找高亮显示
```

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
 

# 防火墙
```bash
#查看状态
firewall-cmd --state 
# 开启防火墙
systemctl start firewalld.service 
# 关闭防火墙
systemctl stop firewalld.service  
# 重启
firewall-cmd --reload  

#禁止firewall开机启动
systemctl disable firewalld.service
#开机启用 
systemctl enable firewalld  
```