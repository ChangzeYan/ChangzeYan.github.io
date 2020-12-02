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

# spring boot 整合activiti默认关闭历史表，手动开启历史表如下：
spring.activiti.history-level=audit
spring.activiti.db-history-used=true

# 可选
#流程定义bpmn放置路径
spring.activiti.process-definition-location-prefix=classpath:/process/
#项目随着spring启动自动部署
spring.activiti.check-process-definitions=true

```

## 改正上述activiti版本创建数据表的bug
启动应用创建表后，运行：
```sql
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- 创建用户表
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) DEFAULT NULL COMMENT '姓名',
  `address` varchar(64) DEFAULT NULL COMMENT '联系地址',
  `username` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin DEFAULT NULL COMMENT '账号',
  `password` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin DEFAULT NULL COMMENT '密码',
  `roles` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin DEFAULT NULL COMMENT '角色',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

-- ----------------------------
-- 填充用户表
-- ----------------------------
INSERT INTO `user` VALUES ('1', 'admincn', 'beijing', 'admin', '$2a$10$gw46pmsOVYO.smHYQ2jH.OoXoe.lGP8OStDkHNs/E74GqZDL5K7ki', 'ROLE_ACTIVITI_ADMIN');
INSERT INTO `user` VALUES ('2', 'bajiecn', 'shanghang', 'bajie', '$2a$10$gw46pmsOVYO.smHYQ2jH.OoXoe.lGP8OStDkHNs/E74GqZDL5K7ki', 'ROLE_ACTIVITI_USER,GROUP_activitiTeam,g_bajiewukong');
INSERT INTO `user` VALUES ('3', 'wukongcn', 'beijing', 'wukong', '$2a$10$gw46pmsOVYO.smHYQ2jH.OoXoe.lGP8OStDkHNs/E74GqZDL5K7ki', 'ROLE_ACTIVITI_USER,GROUP_activitiTeam');
INSERT INTO `user` VALUES ('4', 'salaboycn', 'beijing', 'salaboy', '$2a$10$gw46pmsOVYO.smHYQ2jH.OoXoe.lGP8OStDkHNs/E74GqZDL5K7ki', 'ROLE_ACTIVITI_USER,GROUP_activitiTeam');

-- ----------------------------
-- 修复Activiti7的M4版本缺失字段Bug
-- ----------------------------
alter table ACT_RE_DEPLOYMENT add column PROJECT_RELEASE_VERSION_ varchar(255) DEFAULT NULL;
alter table ACT_RE_DEPLOYMENT add column VERSION_ varchar(255) DEFAULT NULL;

-- ----------------------------
-- 动态表单数据存储
-- ----------------------------
DROP TABLE IF EXISTS `formdata`;
CREATE TABLE `formdata` (
  `PROC_DEF_ID_` varchar(64) DEFAULT NULL,
  `PROC_INST_ID_` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL,
  `FORM_KEY_` varchar(255) DEFAULT NULL,
  `Control_ID_` varchar(100) DEFAULT NULL,
  `Control_VALUE_` varchar(2000) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

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
参考：[自动服务任务](https://www.pianshen.com/article/1950323381/)
[服务任务](https://www.cnblogs.com/dengjiahai/p/6942376.html)
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

## 查询当前流程实例已完成任务
其实是查询act_hi_actinst表，按照时间排序，返回最后完成的活动。
```java
public String getLastFinishedTask(String instanceId){
    List<HistoricActivityInstance> list=historyService.createHistoricActivityInstanceQuery()
            .processInstanceId(instanceId).finished().orderByHistoricActivityInstanceStartTime().desc().list();
    String lastFinishedTask = "空任务";
    if(list!=null && list.size()>0){
        lastFinishedTask=list.get(0).getActivityName();
        for(HistoricActivityInstance taskInstance:list){
            System.out.println("已完成："+taskInstance.getActivityName());
        }
    }else{
        System.out.println("---------------当前任务为空-------------------");
    }
    return lastFinishedTask;
}
```

## 监听类中注入bean
实现JavaDelegate的service task监听类中，采用Autowired注入bean为null，需要BeanFactoryPostProcessor获取。

```java
import org.springframework.aop.framework.AopContext;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.NoSuchBeanDefinitionException;
import org.springframework.beans.factory.config.BeanFactoryPostProcessor;
import org.springframework.beans.factory.config.ConfigurableListableBeanFactory;
import org.springframework.stereotype.Component;

/**
 * spring工具类 方便在非spring管理环境中获取bean
 *
 * @author aaa
 */
@Component
public final class SpringUtils implements BeanFactoryPostProcessor
{
    /** Spring应用上下文环境 */
    private static ConfigurableListableBeanFactory beanFactory;

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException
    {
        SpringUtils.beanFactory = beanFactory;
    }

    /**
     * 获取对象
     *
     * @param name
     * @return Object 一个以所给名字注册的bean的实例
     * @throws org.springframework.beans.BeansException
     *
     */
    @SuppressWarnings("unchecked")
    public static <T> T getBean(String name) throws BeansException
    {
        return (T) beanFactory.getBean(name);
    }

    /**
     * 获取类型为requiredType的对象
     *
     * @param clz
     * @return
     * @throws org.springframework.beans.BeansException
     *
     */
    public static <T> T getBean(Class<T> clz) throws BeansException
    {
        T result = (T) beanFactory.getBean(clz);
        return result;
    }

    /**
     * 如果BeanFactory包含一个与所给名称匹配的bean定义，则返回true
     *
     * @param name
     * @return boolean
     */
    public static boolean containsBean(String name)
    {
        return beanFactory.containsBean(name);
    }

    /**
     * 判断以给定名字注册的bean定义是一个singleton还是一个prototype。 如果与给定名字相应的bean定义没有被找到，将会抛出一个异常（NoSuchBeanDefinitionException）
     *
     * @param name
     * @return boolean
     * @throws org.springframework.beans.factory.NoSuchBeanDefinitionException
     *
     */
    public static boolean isSingleton(String name) throws NoSuchBeanDefinitionException
    {
        return beanFactory.isSingleton(name);
    }

    /**
     * @param name
     * @return Class 注册对象的类型
     * @throws org.springframework.beans.factory.NoSuchBeanDefinitionException
     *
     */
    public static Class<?> getType(String name) throws NoSuchBeanDefinitionException
    {
        return beanFactory.getType(name);
    }

    /**
     * 如果给定的bean名字在bean定义中有别名，则返回这些别名
     *
     * @param name
     * @return
     * @throws org.springframework.beans.factory.NoSuchBeanDefinitionException
     *
     */
    public static String[] getAliases(String name) throws NoSuchBeanDefinitionException
    {
        return beanFactory.getAliases(name);
    }

    /**
     * 获取aop代理对象
     *
     * @param invoker
     * @return
     */
    @SuppressWarnings("unchecked")
    public static <T> T getAopProxy(T invoker)
    {
        return (T) AopContext.currentProxy();
    }
}
```
然后用SpringUtils.getBean(注入类.class)获取。
```java
IActivitiGraph activitiGraph=SpringUtils.getBean(IActivitiGraph.class);
代替
@Autowired
IActivitiGraph activitiGraph;
```
