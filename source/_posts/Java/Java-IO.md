---
title: Java-IO
author: ChangzeYan
date: 2021-01-05 22:40:25
tags: IO
categories: Java
cover:
---

# 读文件
一行一行地读：
```java
public String readFile(){
    String path = "";
    File file = new File(path);
    StringBuilder result = new StringBuilder();
    try{
        BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(file), "UTF-8"));//构造一个BufferedReader类来读取文件

        String s = null;
        while((s = br.readLine())!=null){//使用readLine方法，一次读一行
            result.append( System.lineSeparator() + s);
        }
        br.close();
    }catch(Exception e){
        e.printStackTrace();
    }
    return result.toString();
 }

```
