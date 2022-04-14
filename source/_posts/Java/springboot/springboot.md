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

## 接收参数
参考：https://blog.csdn.net/weixin_43275578/article/details/106052236
json类型：
```
public String login(@RequestBody Map<String,Object> map){
    System.out.println(map);
    return "200";
}
```



# 解决前端请求跨域问题
参考：https://www.cnblogs.com/codegzy/p/15252637.html
```

1.使用@CrossOrigin注解

@RestController
@RequestMapping("demos")
@CrossOrigin
public class DemoController {
    @GetMapping
    public String demos() {
        System.out.println("========demo=======");
        return "demo ok";
    }
}

2.全局解决跨域问题
新建CorsConfig类
@Configuration
public class CorsConfig {
    @Bean
    public CorsFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.addAllowedOrigin("*"); // 1允许任何域名使用
        corsConfiguration.addAllowedHeader("*"); // 2允许任何头
        corsConfiguration.addAllowedMethod("*"); // 3允许任何方法（post、get等）
        source.registerCorsConfiguration("/**", corsConfiguration);//4处理所有请求的跨域配置
     return new CorsFilter(source);
    }
}
```
