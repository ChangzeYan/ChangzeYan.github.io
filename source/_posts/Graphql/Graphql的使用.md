---
title: Graphql的使用
author: ChangzeYan
date: 2020-12-10 20:20:08
tags:
categories: Graphql
cover:
---

# 添加pom依赖
```xml
<dependency>
	<groupId>com.graphql-java</groupId>
	<artifactId>graphql-java-tools</artifactId>
	<version>5.2.4</version>
</dependency>
<dependency>
	<groupId>org.mountcloud</groupId>
	<artifactId>graphql-client</artifactId>
	<version>1.1</version>
</dependency>
<dependency>
	<groupId>com.graphql-java</groupId>
	<artifactId>graphiql-spring-boot-starter</artifactId>
	<version>5.0.2</version>
</dependency>
<dependency>
	<groupId>com.graphql-java</groupId>
	<artifactId>graphql-spring-boot-starter</artifactId>
	<version>5.0.2</version>
</dependency>
```
graphiql便于调试接口和查看文档，项目启动后访问：http://ip:port/graphiql

# 添加Query和Type
在resources根目录下新建root.graphqls文件和schema.graphqls 文件，名字不可更换
## root.graphqls
root.graphqls中用于定义接口：
```java
type Query{
    # 启动任务
    startEnterpriseRelationshipAnalyse(module:String,organizationName:String,organizationUrl:String,userId:String): StartProcessResult
    # 获取当前正在执行的任务名称
    currentTask(processInstanceId: String): ProcessCurrentTaskResult

    # 获取所有任务
    taskList(page:Int, size:Int ): OrganizationTaskPageResult

    # 获取所有已经完成的任务
    completedTaskList(page:Int, size:Int ):OrganizationTaskPageResult
}
```
在graphqls文件中写的注释会体现在http://ip:port/graphiql 中的文档上。
该文件中定义了4个接口，小括号内是接口需要传入的参数，冒号后面是接口的返回值。
- Result：返回一个Result的数据类型
-  \[AnalysisJsonResponse\]，返回一个 AnalysisJsonResponse类型的列表。
- ProcessCurrentTaskResult返回一个ProcessCurrentTaskResult类型

Result和 ProcessCurrentTaskResult等类型都在schema.graphqls中定义。

## schema.graphqls
```java
type Result{
    code: Int
    msg: String
}

type StartProcessResult {
    result: Result
    taskId: String
    processInstanceId: String
}

# 获取当前正在执行的任务名称结果
type ProcessCurrentTaskResult{
    result: Result
    processTaskId: String
}


type PageInfo{
    total: Int
    current: Int
    size: Int
}
type OrganizationTaskResult{
    taskId: String
    processCode:String
    processId:String
    userId:String
    organizationName: String
    organizationUrl: String
    module: String
    progress: String
    status: String
    createTime: String
    endTime: String
}

type OrganizationTaskPageResult{
    # [OrganizationTaskResult]表示 OrganizationTaskResult 类型的列表
    list: [OrganizationTaskResult]
    pageInfo: PageInfo
    result: Result
}
```
schema中定义的类型要编写对应的Entity类，方便后面构建对象。
# 定义Resolver
在root.graphqls中定义的接口要在resolver中实现，否则会报错。
```java
import com.coxautodev.graphql.tools.GraphQLQueryResolver;
import com.topveda.cloudactiviti.entity.*;
import com.topveda.cloudactiviti.service.EnterpriseRaService;

import com.topveda.cloudactiviti.service.OrganizationTaskService;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class ActivitiResolver implements GraphQLQueryResolver {
    @Autowired
    private EnterpriseRaService enterpriseRaService;
    @Autowired
    private OrganizationTaskService organizationTaskService;


    public String deploy(){
        String deploymentId=enterpriseRaService.deploy();
        return deploymentId;
    }

    public StartProcessResult startEnterpriseRelationshipAnalyse(String module, String organizationName, String organizationUrl, String userId){
        //....
        return StartProcessResult.builder()
                .taskId(taskId)
                .processInstanceId(processInstanceId)
                .result(Result.builder().code(0).msg("success").build())
                .build();
    }


    public ProcessCurrentTaskResult currentTask(String processInstanceId){
        String currentTaskId=enterpriseRaService.getCurrentTask(processInstanceId);
        return ProcessCurrentTaskResult.builder()
                .result(Result.builder().code(0).msg("success").build())
                .processTaskId(currentTaskId)
                .build();

    }

    public ProcessLastTaskResult lastFinishedTask(String processInstanceId){
        String lastFinishedTaskId=enterpriseRaService.getLastFinishedTask(processInstanceId);
        return ProcessLastTaskResult.builder()
                .result(Result.builder().code(0).msg("success").build())
                .processTaskId(lastFinishedTaskId)
                .build();
    }
    public OrganizationTaskPageResult taskList(int page,int size){
        return organizationTaskService.getTaskListByPage(page,size);
    }

    // 获取所有已经完成的任务
    public OrganizationTaskPageResult completedTaskList(int page,int size){
        return organizationTaskService.getCompletedTaskListByPage(page,size);
    }

}
```

# 调用接口
运行程序，访问http://ip:port/graphiql：
![graphiql测试接口](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/graphiql.png)
打开右侧的query，会看到接口文档，有接口名称，参数和返回值类型
可以根据自己需要的返回值定义访问形式，图中只返回result的msg属性，如果需要返回result的code属性，写为：
```bash
currentTask(processInstanceId:"e00ca85a-360e-11eb-9ca6-809599575c52"){
  result{
    msg
    code
  }
  processTaskId
}
```

## 返回自定义对象列表
如果需要返回自定义的对象列表，以上面定义的completedTaskList为例：
```bash
{
  completedTaskList(page:1,size:10){
    list{
      taskId
      processCode
    }
    pageInfo{
      total
      current
      size
    }
    result{
      code
    }
  }
}
```
