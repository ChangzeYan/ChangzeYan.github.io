---
title: react-hello
author: ChangzeYan
date: 2022-04-14 19:57:53
tags:
categories:
cover:
description:
---

# Js操作json
字符串转json：
```
let str1 = '{ "name": "cxh", "sex": \{"a":"b"\} }';
let jsonObj=JSON.parse(str1);
console.log(jsonObj.name);
console.log(jsonObj.sex);
console.log(jsonObj.sex.a);
```

json对象转为字符串：
```
var last=JSON.stringify(jsonObj); //将JSON对象转化为JSON字符
```

