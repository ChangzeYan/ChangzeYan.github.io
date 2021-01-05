---
title: windows cmd
author: ChangzeYan
date: 2020-12-08 22:31:00
tags: cmd
categories: Windows
cover:
---

## 查看并解除端口占用
查看占用4000端口的进程：
```bash
netstat -ano|findstr "4000"
```

停止进程，PID为LISTENING后的数字）
```bash
taskkill /pid "PID" /F
```
如果出现：“无法终止 PID 为 xxx 的进程”，用管理员方式打开cmd，再次终止进程。


## 打开防火墙端口
在管理员模式下，打开2375端口，支持远程访问：
```bash
netsh advfirewall firewall add rule name="docker_daemon" dir=in action=allow protocol=TCP localport=2375
```
