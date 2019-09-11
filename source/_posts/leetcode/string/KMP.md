---
title: KMP
author: ChangzeYan
date: 2019-08-26 16:49:56
tags:
  - leetcode
  - string
categories: leetcode
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
此时，模式串到达字符Y处失配，**证明模式串中Y之前的字符都已匹配成功**，注意到，前缀字符串*ABCDABC*的共同最长前后缀为*ABC*，此时，目标串的前缀*ABC*和模式串的前缀*ABC*对应，目标串的后缀*ABC*和模式串的后缀*ABC*对应，失配后，模式串的指针跳到*D*处

```yaml
...ABCDABCX       --> 目标串S
       ABCDABCY   --> 模式串P
```

此时, 模式串中*D* 之前的字符 *ABC* 仍是匹配的，因为，此时目标串的后缀 *ABC* 和 模式串的前缀 *ABC* 相匹配。


### Next数组的求解方法

#### 方法1
根据字符串的首尾相同最长子串

#### 方法2



### Next数组求解

```python
def get_next(str_p):
    next_list = [-1]
    j, k = 0, -1
    # next_list 初始化时已经添加了一个-1，所以 判断条件小于 len(str_p)-1
    while j < len(str_p) - 1:
        if k == -1 or str_p[j] == str_p[k]:
            k += 1
            j += 1
            next_list.append(k)
        else:
            k = next_list[k]
    # print(next_list)
    return next_list
```


### Kmp算法的时间复杂度
参考：[KMP时间复杂度分析](https://blog.csdn.net/niukai1768/article/details/79579709)

O(m+n)

https://blog.csdn.net/iamyvette/article/details/77433991

https://blog.csdn.net/weixin_38332967/article/details/81944353

https://blog.csdn.net/v_july_v/article/details/7041827
