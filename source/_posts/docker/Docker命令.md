---
title: Docker命令
author: ChangzeYan
date: 2020-12-16 21:02:03
tags: 命令
categories: Docker
cover:
---

```bash
# 下载镜像
docker pull 镜像名
# 查看镜像
docker images
# 删除镜像
docker rmi -f （强制删除参数） 镜像名/id
```
运行容器：
```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
参数：
OPTIONS说明：
```bash
-a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
-d: 后台运行容器，并返回容器ID；
-i: 以交互模式运行容器，通常与 -t 同时使用；
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-P: 随机端口映射，容器内部端口随机映射到主机的高端口
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
--name="nginx-lb": 为容器指定一个名称；名称是唯一的，不可重名
--dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
--dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
-h "mars": 指定容器的hostname；
-e username="ritchie": 设置环境变量；
--env-file=[]: 从指定文件读入环境变量；
--cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
-m :设置容器使用内存最大值；
--net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
--link=[]: 添加链接到另一个容器；
--expose=[]: 开放一个端口或一组端口；
--volume , -v: 绑定一个卷
--rm：--rm选项不能与-d同时使用，在容器退出后，自动执行docker rm -v
```

进入容器：
```bash
docker exec -it 容器id /bin/bash
```
退出容器（容器不会停止）
```bash
exit
```

```bash
# 删除容器
docker rm 容器id或容器name
# 强制删除容器db01、db02
docker rm -f db01 db02
```

# 查看容器日志
```bash
docker logs [options] 容器名

# 打印容器mytest应用后10行的内容：
docker logs --tail="10" mytest
```
| 名字             | 默认值 | 描述                                 |
|----------------|-----------|------------------------------------|
| –details       |     | 显示提供给日志的额外细节                       |
| –follow或-f     |     | 按日志输出                              |
| –since         |     | 从某个时间开始显示，例如2013-01-02T13:23:37    |
| –tail          | all  | 从日志末尾多少行开始显示                       |
| –timestamps或-t |     | 显示时间戳                              |
| –until         |     | 打印某个时间以前的日志，例如 2013-01-02T13:23:37 |