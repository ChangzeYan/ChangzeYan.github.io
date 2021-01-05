---
title: Docker安装与配置
author: ChangzeYan
date: 2020-12-02 15:32:43
tags:
categories:
  - Docker
cover:
---

# Linux上的安装与配置
参考：[CentOS7上安装docker](https://www.cnblogs.com/yufeng218/p/8370670.html)

## 准备
Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
1、通过 uname -r 命令查看你当前的内核版本（若不满足要求可以先升级内核）
```bash
 uname -r
 ```

2、使用 root 权限登录 Centos。确保 yum 包更新到最新。
```bash
$ sudo yum update
```

3、卸载旧版本的docker(如果安装过旧版本的话)
```bash
$ sudo yum remove docker  docker-common docker-selinux docker-engine
```

4、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```bash
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

5、设置yum源
```bash
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
可以查看所有仓库中所有docker版本，并选择特定版本安装
```bash
$ yum list docker-ce --showduplicates | sort -r
```
## 安装
由于repo中默认只开启stable仓库，故这里安装的是最新稳定版
```bash
sudo yum install docker-ce
```
或者安装指定的版本：
```bash
sudo yum install <FQPN>
# 例如：sudo yum install docker-ce-17.12.0.ce
```

## 启动
启动并加入开机启动
```bash
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
```bash
$ docker version
```
如果有报错：
```bash
Transaction check error:
  file /usr/bin/docker from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd-shim from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/dockerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
```
卸载旧版本的包
```bash
$ sudo yum erase docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
```
再次安装：
```bash
 yum install docker-ce
```

## 配置国内镜像
进入 /etc/docker
```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
```

输入： 网易镜像
```bash
{
    "registry-mirrors":["http://hub-mirror.c.163.com"]
}

EOF
```
重启docker
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 开启远程访问
修改Docker配置文件
```bash
 vi /lib/systemd/system/docker.service　
```

修改ExecStart为：
```bash
ExecStart=/usr/bin/dockerd --containerd=/run/containerd/containerd.sock
```
![配置远程访问](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/docker-安装docker-远程访问.png)

修改daemon.json
```bash
vi /etc/docker/daemon.json
```

添加键值对
```bash
 "hosts": ["0.0.0.0:2375","unix:///var/run/docker.sock"]
```
重启docker
```bash
systemctl daemon-reload
systemctl restart docker
```

**开启远程访问后可以在远程主机操作docker**
要求：操作和被操作的主机都要安装docker
```bash
# 查看镜像
docker -H IP:2375 images
# 查看运行中的容器
docker -H IP:2375 ps
```

## 升级docker
卸载docker
```bash
sudo yum remove $(rpm -qa | grep docker)
```

下载最新版本docker
```bash
curl -fsSL https://get.docker.com/ | sh
```

重启docker
```bash
sudo systemctl restart docker # centos 7
```


再次查看docker版本
```bash
$ docker -v
Docker version 18.09.3, build 774a1f4
```


# windows 安装docker

## 专业版开启Hyper-V功能
windows专业版要在打开或关闭windows功能那里开启Hyper-V功能。

## 下载docker：
[官网](https://www.docker.com/get-started)
安装后，配置环境变量，将:
```bash
C:\\Program Files\\Docker\\Docker\\resources
```
添加到path中。

## 配置国内镜像

![配置国内镜像](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/docker-安装docker-windows国内镜像.png)

## 配置远程访问
参考：[Windows开启Docker远程访问](http://baijiahao.baidu.com/s?id=1652188442217820964&wfr=spider&for=pc)

先勾选：
![配置Windows远程访问](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/docker-安装docker-windows开启远程访问.png)
开启windows防火墙，在管理员模式下：

```bash
netsh advfirewall firewall add rule name="docker_daemon" dir=in action=allow protocol=TCP localport=2375
```
