---
title: 整数反转
date: 2019-06-13 14:54:42
categories: leetcode
cover: true
tags:
  - leetcode
  - cpp_stl
---

## 整数反转
题目地址：[整数反转](https://leetcode-cn.com/problems/reverse-integer/)

题目描述：
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例1：
```
输入: 123
输出: 321
```
示例 2:
```
输入: -123
输出: -321
```
示例 3:
```
输入: 120
输出: 21
```
注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。


### cpp
int 的范围
```
[-2^31,2^31-1]
```
C中常量INT_MAX和INT_MIN分别表示最大、最小整数，定义在头文件limits.h中。
```cpp
INT_MAX = 2147483647(2^31-1)
INT_MIN = -2147483648(-2^31)
```

### 思路
利用x%10循环取出原数最后一位，作为reverse_num的首位
```cpp
long long reverse_num=0; //用long long定义反转后的结果
while (x) {
    // 不用考虑x的正负，负数的模为负
    current=x%10; //每次取出x的最后一位，将其当作反转数的第一位
    reverse_num=reverse_num*10+current;
}

```

### 代码
```cpp
class Solution {
public:
    int reverse(int x) {
        long long num=0;
        while(x){
            int current_num=x%10;
            cout<<current_num<<endl;
            num=num*10+current_num;
            if(num<INT_MIN ||num>INT_MAX){
                return 0;
            }
            x/=10;
        }
        //cout<<num<<endl;
        return num;
    }
};
```
