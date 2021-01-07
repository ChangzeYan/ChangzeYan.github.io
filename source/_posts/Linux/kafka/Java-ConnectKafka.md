---
title: Java-ConnectKafka
author: ChangzeYan
date: 2021-01-07 09:58:00
tags: Java-Kafka
categories: Kafka
cover:
---

## 从头开始消费

消费者要从头开始消费某个topic的全量数据，需要满足2个条件（spring-kafka）：
- 使用一个全新的"group.id"（就是之前没有被任何消费者使用过）;
- 指定"auto.offset.reset"参数的值为earliest；