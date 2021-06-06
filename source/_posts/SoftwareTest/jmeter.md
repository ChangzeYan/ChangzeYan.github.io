---
title: jmeter
author: ChangzeYan
date: 2021-06-05 20:03:46
tags: 
categories: SoftwareTest
cover:
---


# Jmeter
参考：[JMeter性能测试，完整入门篇](https://blog.csdn.net/u012111923/article/details/80705141)
## 安装
[下载地址](http://jmeter.apache.org/download_jmeter.cgi)

执行：
解压后，执行bin下面的jmeter.bat即可 

### 安装jp@gc等插件
参考：[Jmeter-监听器jp@gc](https://blog.csdn.net/LXiaYu123456789/article/details/113694985)

使用监听器jp@gc的，需要选项中存在Plugins Manager,如果没有请下插件：[Plugins Manager](https://jmeter-plugins.org/downloads/all/)

![JmeterPlugin](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/jmeterplugin.png)

下载后放置在jmeter安装目录下的lib/ext中，重新启动即可

如下图，安装jpgc插件：
![JmeterPlugin](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/jmeterjpgc.png)


## 生成web页面的报告
```
jmeter -n -t D:\program\apache-jmeter-5.0\samples\28.summary_report.jmx -l d:\summary.jtl -e -o d:\result
```

jmx文件：jmeter的测试项目文件
jtl文件：生成的脚本文件
d:\result：web页面报告的存储路径（必须为空目录）