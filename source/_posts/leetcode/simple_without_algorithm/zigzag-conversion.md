---
title: Z字形变换
date: 2019-06-13 14:54:42
categories:
  - leetcode
cover: true
tags:
  - leetcode
  - simple_without_algorithm
---
## Z字形变换

题目地址：[Z字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

### 题目描述
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
```
L   C   I   R
E T O E S I I G
E   D   H   N
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
示例 1:
```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

示例 2:
```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

### 思路：

画出变换后字母的图形，将每个字母的行号写在它右边
```
L1    C1    I1    R1
E2 T2 O2 E2 S2 I2 I2 G2
E3    D3    H3    N3
```

将每个字母变换后对应的行号与该字母对应：
```
LEETCODEISHIRING
1232123212321232
```
便可以得到字母变换后行号的规律

### 代码
```cpp
// 保存字母与其行号对应关系的结构体
struct Node{
    int row;
    char c;
};
class Solution {
public:
    string convert(string s, int numRows) {
        if(s.empty()){
            return "";
        }
        if(numRows==1){
            return s;
        }
        string result_str;
        // 初始化行号
        int ptr=1;
        // 决定行号是增加还是减少
        bool flag=true;
        int len=s.length();
        Node nodeList[len];
        for(int i=0;i<len;i++){
            Node n{};
            n.c=s[i];
            n.row=ptr;
            if(flag){
                ptr++;
            }else{
                ptr--;
            }
            if(ptr==numRows || ptr==1){
                flag=!flag;
            }
            nodeList[i]=n;
        }
        string z_character[numRows+1];
        for(int i=0;i<len;i++){
    //        cout<<nodeList[i].row<<" "<<nodeList[i].c<<endl;
            char c=nodeList[i].c;
            int row=nodeList[i].row;
            z_character[row]+=c;
        }
        for(const string& str:z_character){
            result_str+=str;
        }
    //    cout<<result_str<<endl;
        return result_str;
    }
};
```
