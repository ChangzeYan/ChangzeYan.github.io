---
title: 工作流
author: ChangzeYan
date: 2020-11-10 14:22:22
tags: activiti
categories:
  - 工作流
cover:
---

# 工作流-Activiti

## bpmn插件
- [activiti6.0的绘图编辑器操作、使用、汉化](https://blog.csdn.net/qq_33333654/article/details/101202362)
- vscode 插件：bpmn editor

## Springboot 依赖
```xml
<dependency>
	<groupId>org.activiti</groupId>
	<artifactId>activiti-spring-boot-starter</artifactId>
	<version>7.1.0.M4</version>
</dependency>
<dependency>
	<groupId>org.activiti.dependencies</groupId>
	<artifactId>activiti-dependencies</artifactId>
	<version>7.1.0.M4</version>
	<type>pom</type>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```
配置数据库连接

```sql
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/db_activiti?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=UTC&nullCatalogMeansCurrent=true&allowPublicKeyRetrieval=true
spring.datasource.username=root
spring.datasource.password=xxxx
```

## 部署和执行
```java
@Service
public class EnterpriseRaService {
    @Autowired
    private RepositoryService repositoryService;

    @Autowired
    private RuntimeService runtimeService;

    public String deploy(){
        Deployment deployment=repositoryService.createDeployment()
                .addClasspathResource("processes/analyse1.bpmn")
                .name("请假流程")
                .deploy();
        return deployment.getId();
    }

    public String startProcess(){
        ProcessInstance instance=runtimeService.startProcessInstanceByKey("process_analyse");
        return instance.getId();
    }
}
```

## Service Task

service task需要在bpmn文件中配置与之关联的执行类，async设置为true表示startProcessInstanceByKey方法立即返回，然后异步执行service task；
```xml
 <bpmn2:serviceTask id="Activity_0z1g1ix" name="crawl" activiti:async="true" activiti:class="com.example.cloudactiviti.listener.CrawlListener">
```
与之关联的类实现JavaDelegate接口：
```java
@Component
public class CrawlListener implements JavaDelegate {

    @Override
    public void execute(DelegateExecution delegateExecution) {
        System.out.println("----------------------");
        System.out.println("-----模拟爬虫-------");
        try {
            for (int i = 0; i < 10; i++) {
                System.out.println(i);
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("end");
    }
}
```

然后调用startProcessInstanceByKey执行流程，该方法立即返回，流程异步执行。
