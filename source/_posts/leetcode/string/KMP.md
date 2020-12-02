---
title: KMP
author: ChangzeYan
date: 2019-08-26 16:49:56
tags:
  - leetcode
  - string
categories: Leetcode
cover: true
---

## KMP

### Next数组的含义
记录当前字符**前缀字符串**的最长前缀后缀的长度，记录了模式串在当前位置失配后，模式串指针*j*指向的下一个位置，即最大相同前缀的下一个字符

### Next数组降低时间复杂度的原理
参考：
- [Kmp中next数组含义](https://blog.csdn.net/TesuZer/article/details/81031351)

- [从头到尾彻底理解KMP](https://blog.csdn.net/v_july_v/article/details/7041827)

```yaml
...ABCDABCX  --> 目标串S
   ABCDABCY  --> 模式串P
```
此时，模式串到达字符Y处失配，**证明模式串中Y之前的字符都已匹配成功**，注意到，前缀字符串*ABCDABC*的共同最长前后缀为*ABC*，此时，**目标串的前缀*ABC*和模式串的前缀*ABC*对应，目标串的后缀*ABC*和模式串的后缀*ABC*对应**，失配后，模式串的指针跳到*D*处

```yaml
...ABCDABCX       --> 目标串S
       ABCDABCY   --> 模式串P
```

此时, 模式串中*D* 之前的字符 *ABC* 仍是匹配的，因为，**此时目标串的后缀 *ABC* 和 模式串的前缀 *ABC* 相匹配**。

![Next数组作用图示](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/KMP_1.jpg)


### Next数组的求解方法

#### 方法1
根据字符串的首尾相同最长子串

#### 方法2

对模式串的标号方式不同，求出的next数组也不相同。模式串如果从0开始标号，求出的next数组比从1标号的next数组每位小1。

##### 从1开始标号：前两位固定为0、1

<table>
    <thead>
        <tr>
            <td class='td'>标号</td>
            <td class='td'>1</td>
            <td class='td'>2</td>
            <td class='td'>3</td>
            <td class='td'>4</td>
            <td class='td'>5</td>
            <td class='td'>6</td>
            <td class='td'>7</td>
            <td class='td'>8</td>
            <td class='td'>9</td>
            <td class='td'>10</td>
            <td class='td'>11</td>
            <td class='td'>12</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class='td'>P</td>
            <td class='td'>a</td>
            <td class='td'>b</td>
            <td class='td'>a</td>
            <td class='td'>b</td>
            <td class='td'>a</td>
            <td class='td'>a</td>
            <td class='td'>a</td>
            <td class='td'>b</td>
            <td class='td'>a</td>
            <td class='td'>b</td>
            <td class='td'>a</td>
            <td class='td'>a</td>
        </tr>
        <tr>
            <td class='td'>Next</td>
            <td class='td'>0</td>
            <td class='td'>1</td>
            <td class='td'>1</td>
            <td class='td'>2</td>
            <td class='td'>3</td>
            <td class='td'>4</td>
            <td class='td'>2</td>
            <td class='td'>2</td>
            <td class='td'>3</td>
            <td class='td'>4</td>
            <td class='td'>5</td>
            <td class='td'>6</td>
        </tr>
    </tbody>
</table>

>字符串的前后定义：`标号小的为前面`

求第3位a的next值，看它前一位字符，为b(**后面一直和b比较，b为目标字符**),其next值为1 ——> 找标号为1的字符，为a，不等于b，但是找到第1位了，则将第3位的next置1

求第4位的next值，看第3位的字符，为a(**后面一直和a比较，a为目标字符**),其next值为*1* --> 找标号为*1*的字符，为a，等于a，**以第3位上的next值作为标号的字符等于a**，所以，**第3位** next值**加1**，作为目标位(第4位)的next值

求第5位的next值，看第4位的字符，为b(**后面一直和b比较，b为目标字符**)，其next值为*2*  --> 找标号为*2*的字符，为b，等于b，**以第4位上的next值作为标号的字符等于b**，所以，**第4位** next值**加1**，作为目标位(第5位)的next值

求第6位的next值，看第5位的字符，为a(**后面一直和a比较，a为目标字符**)，其next值为*3*  --> 找标号为*3*的字符，为a，等于a，**以第5位上的next值作为标号的字符等于a**，所以，**第5位** next值**加1**，作为目标位(第5位)的next值

求第7位的next值，看第6位的字符，为a(**后面一直和a比较，a为目标字符**)，其next值为*4*  --> 找标号为*4*的字符，为b，不等于a，**继续寻找**--> 标号为*4*的next值为 *2* --> 标号为 *2* 的字符为b，不等于a，**继续寻找**  --> 标号 *2* 的next值为 *1* --> 标号为 *1* 的字符为 a，等于a， **以第2位上的next值作为标号的字符等于a**，所以，**第2位** next值**加1**，作为目标位(第7位)的next值

求第8位的next值，看第7位的字符，为a(**后面一直和a比较，a为目标字符**)，其next为 *2*，--> 标号为 *2* 的字符为b，不等于a， **继续寻找**，标号 *2* 的next值为 *1* --> 标号为 *1* 的字符为 a，等于a，**以第2位上的next值作为标号的字符等于a**，所以，**第2位** next值**加1**，作为目标位(第8位)的next值

求第9位的next值，看第8位的字符，为b(**后面一直和b比较，b为目标字符**)，其next值为*2*  --> 找标号为*2*的字符，为b，等于b，**以第8位上的next值作为标号的字符等于b**，所以，**第8位** next值**加1**，作为目标位(第9位)的next值

求第9位的next值，看第8位的字符，为b(**后面一直和b比较，b为目标字符**)，其next值为*2*  --> 找标号为*2*的字符，为b，等于b，**以第8位上的next值作为标号的字符等于b**，所以，**第8位** next值**加1**，作为目标位(第9位)的next值

求第10位的next值，看第9位的字符,为a(**后面一直和a比较，a为目标字符**),其next值为 *3* --> 找标号为*3*的字符，为a，等于a，**以第9位上的next值作为标号的字符等于a**，所以，**第9位** next值**加1**，作为目标位(第10位)的next值

求第11位的next值，看第10位的字符，为b(**后面一直和b比较，b为目标字符**)，其next值为*4*  --> 找标号为*4*的字符，为b，等于b，**以第10位上的next值作为标号的字符等于b**，所以，**第10位** next值**加1**，作为目标位(第11位)的next值

求第12位的next值，看第11位的字符,为a(**后面一直和a比较，a为目标字符**),其next值为 *5* --> 找标号为*5*的字符，为a，等于a，**以第11位上的next值作为标号的字符等于a**，所以，**第11位** next值**加1**，作为目标位(第12位)的next值

##### 从0开始标号：前两位固定为-1、0
与上面的步骤相同，只是模式串的下标不同：

| 标号 |0|1|2|3|4|5|6|7|8|9|10|11|
|:-: | :-: | :-: | :-: | :-:|:-: | :-: | :-: | :-:|:-: | :-: | :-: | :-:|
|P | a|b|a|b|a|a|a|b|a|b|a|a|
|Next|-1|0|0|1|2|3|1|1|2|3|4|5|


### Next数组求解
根据方法2求next数组的代码如下：
```python
def get_next(str_p):
    next_list = [-1]
    # j为模式串下标，k为next值
    j, k = 0, -1
    # next_list 初始化时已经添加了一个-1，所以 判断条件小于 len(str_p)-1
    while j < len(str_p) - 1:
        # k==-1 是判断找没找到第一个字符，j一直是当前字符的前一个字符下标
        # str_p[j] == str_p[k] 判断 以某个字符的next值为标号的对应的字符与当前位前一位字符是否相同
        if k == -1 or str_p[j] == str_p[k]:
            # 如果相同，该位next+1作为目标位的next值
            k += 1
            j += 1
            next_list.append(k)
        else:
            k = next_list[k]
    # print(next_list)
    return next_list
```

### KMP
```python
def kmp(s, p):
    next_list = get_next(p)
    i, j = 0, 0
    while i < len(s) and j < len(p):
        if j == -1 or s[i] == p[j]:
            i += 1
            j += 1
        # 如果失配，目标串指针i不动，模式串指针j跳到失配位的next值处
        # 使得失配位置前缀字符串的后缀对应于模式串的前缀
        else:
            j = next_list[j]
    if j == len(p):
        return i - j
    else:
        return -1
```

### Kmp算法的时间复杂度
参考：[KMP时间复杂度分析](https://blog.csdn.net/niukai1768/article/details/79579709)

O(m+n)

https://blog.csdn.net/iamyvette/article/details/77433991

https://blog.csdn.net/weixin_38332967/article/details/81944353

https://blog.csdn.net/v_july_v/article/details/7041827
