---
title: jsp-basic
author: ChangzeYan
date: 2022-01-16 10:00:51
tags: 
categories: Jsp
cover:
description: Jsp 基础知识
---


# Jsp引入bootstrap样式
参考：https://blog.csdn.net/qq_43254488/article/details/85099244

下载：https://www.bootcss.com/

点某个版本中文文档：https://v3.bootcss.com/
进入下载页

下载用于生产环境的bootstrap

将下载的压缩包解压，在jsp项目webapp目录下新建static文件夹，将解压的文件夹放到static目录下：

![bootstrap文件位置](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/jsp_bootstrap.png)

在jsp页面中引用：
方式1：用basepath
```
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%
    String path=request.getContextPath();
    String bathPath=request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<link rel="stylesheet" type="text/css" href="<%=bathPath%>/static/bootstrap-3.4.1-dist/css/bootstrap.min.css">
<script type="text/javascript" src="<%=bathPath%>/static/bootstrap-3.4.1-dist/js/bootstrap.min.js"></script>
```
方式2：直接引用
head:
```
<link rel="stylesheet" type="text/css" href="static/bootstrap-3.4.1-dist/css/bootstrap.min.css">
<script type="text/javascript" src="static/bootstrap-3.4.1-dist/js/bootstrap.min.js"></script>
```
body:

```
<nav aria-label="Page navigation">
    <ul class="pagination">
        <li>
            <a href="#" aria-label="Previous">
                <span aria-hidden="true">&laquo;</span>
            </a>
        </li>
        <li><a href="#">1</a> </li>
        <li><a href="#">2</a> </li>
        <li><a href="#">3</a> </li>
        <li><a href="#">4</a> </li>
    </ul>
</nav>

<button type="button" class="btn btn-default">默认样式</button>
<button type="button" class="btn btn-primary">首选项</button>
<button type="button" class="btn btn-success">成功</button>

```