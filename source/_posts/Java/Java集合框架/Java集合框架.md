---
title: Java集合框架
author: ChangzeYan
date: 2020-12-19 20:24:04
tags: Collection
categories: Java
cover:
---

# 数组和集合的区别
- 数组能存放基本数据类型和对象，而集合类存放的都是对象，集合类不能存放基本数据类型。数组和集合存放的对象皆为对象的引用地址。Java集合中实际存放的只是对象的引用，每个集合元素都是一个引用变量，实际内容都放在堆内存或者方法区里。

```java
// 数组能存放对象和基本数据类型
int[] nums=new int[10];
List[] listNums=new List[10];

// 集合只能存放引用数据类型
List<Integer>list=new LinkedList<>();
```

- 数组大小固定无法动态改变，集合类容量动态改变。
- 数组无法判断其中实际存有多少元素，length只告诉了数组的容量，而集合的size()可以确切知道元素的个数
- 集合有多种实现方式和不同适用场合，不像数组仅采用顺序表方式
- 集合以类的形式存在，具有封装、继承、多态等类的特性，通过简单的方法和属性即可实现各种复杂操作，大大提高了软件的开发效率，而数组不具备这种特征

# Java集合框架图
椭圆为接口，方形为类，实线为继承，虚线为实现：
![Java集合框架图](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/Java集合框架图.png)
