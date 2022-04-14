---
title: mongo-java
author: ChangzeYan
date: 2022-04-12 01:05:41
tags: 
categories:
cover:
description: 
---

# 连接

> springboot 依赖和mongo-java-driver不能同时使用
```
springboot:
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>

application.properties中配置：
含密码：
spring.data.mongodb.uri=mongodb://用户名:密码@localhost:27017/数据库名
不含密码：
mongodb://服务器IP:端口/数据库名

java:
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongo-java-driver</artifactId>
    <version>3.12.10</version>
</dependency>

```
java:
```
String mongoUrl="mongodb://用户名:密码@localhost:27017/数据库名";
MongoClientURI mongoClientURI=new MongoClientURI(mongoUrl);
MongoClient client=new MongoClient(mongoClientURI);
MongoDatabase database=client.getDatabase("数据库名");
MongoCollection<Document>collection=database.getCollection("集合名");
查询全部结果：
FindIterable<Document>findIterable=collection.find();
for (Document document : findIterable) {
    System.out.println(document);
}
```


# 增删改查
映射方式操作：
预定义一些映射的实体类，然后打上@Ducument注解。在使用MongoTemplate的查询文档的时候，一般就是根据查询语句创建一个Query对象，然后用MongoTemplate.find(query, entityClass)这样的方式，就可以返回对应的实体对象集合

非映射方式：
　非映射方式操作Document，就是不需要预定义实体类，也就是没有实体类。我们只需要JSON数据就可以了。而刚好Document对象有提供toJson方法，可以返回一个JSON字符串。非映射方式不能直接用MongoTemplate直接find，而是要先获取集合对象，然后，在集合内部，相当于在表内部查询。

```
public String findTest() {
    //1.获取集合对象
    MongoCollection<Document> collection = mongoTemplate.getCollection("test");
    //2.创建用于查询的BSON对象,eq()方法是Mongo Spring Boot整合包提供的
    Bson bson = eq("field", "value");
    //3.利用bson条件查询结果  import org.bson.Document;
    FindIterable<Document> documents = collection.find(bson);
    //4.将结果拼接成json数组
    StringBuilder sb = new StringBuilder();
    sb.append("[");
    for(Document document : documents) {
        sb.append(document.toJson() + ",\n");
    }
    sb.append("]");
    return sb.toString();
}
```


## 条件查询
mongo语句：
查询weeklog集合中 week-list对象列表中，time属性为“2022年4月11日-2022年4月17日周报”的对象
```
db.getCollection("weeklog").find({
	"week-list":{
		$elemMatch:{
			"time":{$eq:"2022年4月11日-2022年4月17日周报"}
		}
	}
})
```

Java：
```
MongoCollection<Document>collection=database.getCollection("weeklog");

BasicDBObject basicDBObject=new BasicDBObject("week-list",new BasicDBObject("$elemMatch",new BasicDBObject("time",
        new BasicDBObject("$eq","2022年4月11日-2022年4月17日周"))));

FindIterable<Document>findIterable=collection.find(basicDBObject);
for (Document document : findIterable) {
    System.out.println(document);
}
```

spring:
参考：https://wenku.baidu.com/view/e436c2074b2fb4daa58da0116c175f0e7cd119c1.html
```
MongoCollection<Document> mongoCollection=mongoTemplate.getCollection("weeklog");
FindIterable<Document> data=mongoCollection.find();
Criteria criteria=Criteria.where("week-list").elemMatch(Criteria.where("time")).is("2022年4月11日-2022年4月17日周报");
Query query=new Query(criteria);
List<WeekLog> res=mongoTemplate.find(query, WeekLog.class);
System.out.println(res);
```


# 索引
## 唯一索引
参考：https://haicoder.net/mongodb/mongodb-index-unique.html
https://blog.csdn.net/chechengtao/article/details/106884612

在 MongoDB 中我们创建的索引时，如果希望某个字段的值只能是唯一的，那么我们可以使用唯一索引来约束该字段，在 MongoDB 中，创建唯一索引，可以使用 unique 参数。
worklog:

```
{
    "_id": ObjectId("6256d6275a3400009a007d9c"),
    "time": "2022年4月11日-2022年4月17日周报"
}
```
建立time 唯一索引：
```
db.worklog.createIndex({"time":1}, {unique: true});
```