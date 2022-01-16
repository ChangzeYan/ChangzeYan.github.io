---
title: tomcat
author: ChangzeYan
date: 2022-01-12 18:03:57
tags:
categories: tomcat
cover:
description:
---


# 安装

下载地址：https://tomcat.apache.org/download-80.cgi
下载core版本，64-bit Windows zip (pgp, sha512)

解压后新建环境变量
CATALINA_BASE ，值为根目录：D:\Environment\Tomcat\apache-tomcat-8.5.59

CATALINA_HOME ，值为根目录：D:\Environment\Tomcat\apache-tomcat-8.5.59

打开path，在后面添加：
;%CATALINA_HOME%\lib;%CATALINA_HOME%\bin;

验证：
cmd中输入：startup


# 乱码

1、找到apache-tomcat-7.0.92/conf/logging.properties
2、将java.util.logging.ConsoleHandler.encoding 从utf-8改为GBK

重启