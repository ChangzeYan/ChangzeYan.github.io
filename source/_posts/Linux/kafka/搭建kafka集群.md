---
title: 搭建kafka集群
author: ChangzeYan
date: 2021-01-05 10:27:18
tags: Kafka
categories: Linux
cover:
---

# 搭建zookeeper集群
参考：[centos7安装zookeeper3.4.12集群](https://www.cnblogs.com/lenmom/p/10290045.html)

## 环境准备
三台服务器，已经做了ssh免密登录，都安装了jdk8
```bash
master: 192.168.244.5
slave1: 192.168.244.6
slave2: 192.168.244.7
```

## 下载和解压
[zookeeper历史版本](http://archive.apache.org/dist/zookeeper/)
本次使用版本：zookeeper-3.4.12.tar.gz
在三台服务器都建立 /opt/software目录
```bash
mkdir -p /opt/software
```
将安装包上传到master服务器，解压：
```bash
tar -xzvf zookeeper-3.4.12.tar.gz -C  /opt/software
```

## 配置
```bash
mv /opt/software/zookeeper-3.4.12/conf/zoo_sample.cfg  /opt/software/zookeeper-3.4.12/conf/zoo.cfg  

vi /opt/software/zookeeper-3.4.12/conf/zoo.cfg
```
注释掉dataDir=/tmp/zookeeper，然后在文件末尾添加：
```bash
dataDir=/opt/software/zookeeper-3.4.12/data
dataLogDir=/opt/software/zookeeper-3.4.12/log
# 主机名可以用对应ip代替
server.1=master:2888:3888
server.2=slave1:2888:3888
server.3=slave2:2888:3888
```

创建myid文件：
```bash
mkdir -p /opt/software/zookeeper-3.4.12/data  #创建数据目录，该目录在zoo.cfg中配置
cd /opt/software/zookeeper-3.4.12/data #上面配置的zookeeper数据保存目录
touch myid   #创建myid文件
echo "1">>myid   #往myid中写入1，对应server.X={IP}:2888:3888 中的x数字
```
将上面在master机器上配置好的zookeeper复制到slave1,slave2两台机器上去：
```bash
scp -r /opt/software/zookeeper-3.4.12/ slave1:/opt/software/  
scp -r /opt/software/zookeeper-3.4.12/ slave2:/opt/software/
```

修改slave1,slave2机器上/opt/software/zookeeper-3.4.12/data/myid为对应的值：
slave1中：
```bash
cd /opt/software/zookeeper-3.4.12/data
rm -f ./myid
echo "2">>myid
```
slave2中：
```bash
cd /opt/software/zookeeper-3.4.12/data
rm -f ./myid
echo "3">>myid   #往myid中写入3，对应server.X={IP}:2888:3888 中的x数字,此处为3
```

## 配置环境变量
```bash
vim /etc/profile
```
在PATH后添加：:$ZK_HOME/bin
```bash
export ZK_HOME=/opt/software/zookeeper-3.4.12
export PATH=XXXXXXX:$ZK_HOME/bin
```

使环境变量生效：
```bash
source /etc/profile
```
三台服务器都配置环境变量。

## 启动zookeeper
三台服务器都执行：
```bash
zkServer.sh start
```

## zookeeper命令
```bash
# 启动ZK服务      
sh zkServer.sh start

# 查看ZK服务状态: 
sh zkServer.sh status
# 停止ZK服务: 
sh zkServer.sh stop
# 重启ZK服务
sh zkServer.sh restart
# 客户端连接zookeeper
zkCli.sh -server master:2181
```

# 搭建Kafka
参考：
- [Centos7搭建kafka集群](https://www.cnblogs.com/ifme/p/13929928.html)
- [centos7搭建kafka集群](https://www.cnblogs.com/xiaohan970121/p/12357199.html)

## 下载
[历史版本](http://archive.apache.org/dist/kafka)
本文选择kafka_2.13-2.4.0.tgz。
上传到master /opt/software下，解压：
```bash
tar -zxvf kafka_2.12-2.6.0.tgz
```


## 配置

修改server.properties:
```bash
vi /opt/software/kafka_2.13-2.4.0/config/server.properties

# 修改如下内容
// 依次增长的整数，0、1、2，集群中Broker的唯一id
broker.id：0
advertised.listeners=PLAINTEXT://192.168.244.5:9092
zookeeper.connect=192.168.244.5:2181,192.168.244.6:2181,192.168.244.7:2181
```
修改/opt/software/kafka_2.13-2.4.0/config/zookeeper.properties中的dataDir：
```bash
dataDir=/opt/software/zookeeper-3.4.12/data
```
分发到其他两个节点：
```bash
scp -r /opt/software/kafka_2.13-2.4.0/  slave1:/opt/software/
scp -r /opt/software/kafka_2.13-2.4.0/  slave2:/opt/software/
```
修改其它节点配置文件

```bash
#slave1节点
vi /opt/software/kafka_2.13-2.4.0/config/server.properties

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=1
advertised.listeners=PLAINTEXT://192.168.244.6:9092

#slave2节点
vi /opt/software/kafka_2.13-2.4.0/config/server.properties

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=2
advertised.listeners=PLAINTEXT://192.168.244.7:9092
```

## 启动
三台服务器在/opt/software/kafka_2.13-2.4.0/bin/下：
```bash
./kafka-server-start.sh -daemon ../config/server.properties
```

## kafka命令
```bash
# 启动kafka
kafka-server-start.sh -daemon ../config/server.properties

# 停止
./kafka-server-stop.sh

# 查看topic
sh kafka-topics.sh --list --zookeeper master:2181,slave1:2181,slave2:2181

# 创建topic
sh kafka-topics.sh --create --zookeeper master:2181,slave1:2181,slave2:2181 --replication-factor 1 --partitions 1 --topic test

# 发送消息
kafka-console-producer.sh --broker-list master:9092,slave1:9092,slave2:9092 --topic test

# 接收消息
kafka-console-consumer.sh --bootstrap-server  master:9092,slave1:9092,slave2:9092 --topic test --from-beginning
```

# 安装中的问题

## kafka无法启动
master中原来安装过单机版kafka，启动时报错（在/opt/software/kafka_2.13-2.4.0/logs/server.log中）：

```bash
ERROR Fatal error during KafkaServer startup. Prepare to shutdown (kafka.server.KafkaServer) kafka.common.InconsistentClusterIdException: The Cluster ID Reu8ClK3TTywPiNLIQIm1w doesn't match stored clusterId Some(BaPSk1bCSsKFxQQ4717R6Q) in meta.properties. The broker is trying to join the wrong cluster. Configured zookeeper.connect may be wrong. at kafka.server.KafkaServer.startup(KafkaServer.scala:220) at kafka.server.KafkaServerStartable.startup(KafkaServerStartable.scala:44) at kafka.Kafka$.main(Kafka.scala:84) at kafka.Kafka.main(Kafka.scala)
```
参考：[kafka.common.InconsistentClusterIdException)
](https://stackoverflow.com/questions/59481878/unable-to-start-kafka-with-zookeeper-kafka-common-inconsistentclusteridexceptio)

原因是两个kafka占用了同一个kafka日志目录（存放topic的目录）。

### 解决
1. 停止三台服务器中zookeeper和kafka。

2. 删除 三个服务器以下内容：
```bash
/opt/software/kafka_2.13-2.4.0/logs 下所有内容
/opt/software/zookeeper-3.4.12/log 下所有内容
/opt/software/zookeeper-3.4.12/data 下除了myid外所有内容
```

3. 在三台服务器下新建kafka日志目录：
```bash
mkdir -p /opt/software/kafka_2.13-2.4.0/kafka-logs
```
4. 修改/opt/software/kafka_2.13-2.4.0/config/server.properties中配置的kafka日志目录：
```bash
vi /opt/software/kafka_2.13-2.4.0/config/server.properties

# 三台服务器都修改
log.dirs=/opt/software/kafka_2.13-2.4.0/kafka-logs
```
然后重新启动zookeeper和kafka。

## zookeeper ConnectException
启动zookeeper后发现/opt/software/zookeeper-3.4.12/data/zookeeper.out中报错：

![Connection refused](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/zookeeper_connection_err.png)

### 解决
参考：[WARN [WorkerSender[myid=1]:QuorumCnxManager@584] - Cannot open channel to 2 at election address /x.x.x.x:3888](https://www.cnblogs.com/chuijingjing/p/10907244.html)

如果是刚启动zookeeper报出这个错误，然后不再不错，那就是正常现象。是由于有的节点启动，而有的节点还没有启动，这段时间已经启动的节点就会去努力寻找没有启动的节点，就会报出这样的错误。这是一种正常现象，无需多虑。

如果启动很长时间之后还在报错，可以尝试：
修改每个节点的zoo.cfg文件中的相对应的server.x=0.0.0.0:2888:3888


```bash
# master:
server.1=0.0.0.0:2888:3888
server.2=slave1:2888:3888
server.3=slave2:2888:3888

# slave1:
server.1=master:2888:3888
server.2=0.0.0.0:2888:3888
server.3=slave2:2888:3888

# slave2:
server.1=master:2888:3888
server.2=slave1:2888:3888
server.3=0.0.0.0:2888:3888

```