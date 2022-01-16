---
title: springboot
author: ChangzeYan
date: 2021-12-16 20:48:22
tags:
categories:
cover:
description: springboot基础知识
---

# 注解

## controller
@Controller：用于定义控制器类，在spring项目中由控制器负责将用户发来的URL请求转发到对应的服务接口（service层），一般这个注解在类中，通常方法需要配合注解@RequestMapping。

```
@Controller
public class Configurecontroller {
    
}
```

@RestController：用于标注控制层组件(如struts中的action)，@ResponseBody和@Controller的合集。
@RequestMapping：提供路由信息，负责URL到Controller中的具体函数的映射。
