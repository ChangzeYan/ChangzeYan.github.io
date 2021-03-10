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

## 无法访问github
能正常访问其他网站，却访问不了GitHub，怀疑是本地DNS解析出现问题。
访问：[fastly.net.ipaddress.com](https://fastly.net.ipaddress.com/github.global.ssl.fastly.net#ipinfo)

![github.global.ssl.fastly.net 的ip](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/fastly.net.ip.png)

访问：[github 的ip](https://github.com.ipaddress.com/#ipinfo)

![github 的ip](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/github.ip.png)

打开C:\Windows\System32\drivers\etc\hosts，添加：
```bash
#github
140.82.112.3  github.com
199.232.69.194 github.global.ssl.fastly.net   
```

然后在cmd中运行（可选）：
```bash
ipconfig /flushdns 
```