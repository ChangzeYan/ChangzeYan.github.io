---
title: 两数之和-输入BST
author: ChangzeYan
date: 2019-06-15 21:24:40
tags:
  - leetcode
  - tree
categories: leetcode
cover: true
---

## 两数之和-输入BST
题目地址：[两数之和-输入BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

### structure
```yaml
BST: 二叉搜索树，每个节点的值大于其任意左侧子节点的值，小于其任意右节点的值
```


### 题目描述
给定一个二叉搜索树和一个目标结果，如果 `BST` 中存在两个元素且它们的和等于给定的目标结果，则返回 `true`。

案例 1:
```c
输入:
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

输出: True
```

案例 2:
```c
输入:
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

输出: False
```

### 思路
构建树的中序遍历（`BST`的中序遍历为**递增序列**） `list`，设置两个指针，分别指向 `list` 的首尾，若首尾指针之和小于 `Target` ，则首指针后移一位；若首尾指针之和大于 `Target` ，则尾指针前移一位；若首尾指针之和等于 `Target` 则做出标记，退出循环。

### 代码
```python
class Solution(object):
    def __init__(self):
        self.tree_li = []

    # 中序遍历
    def ergodic_tree(self, root):
        if root is not None:
            self.ergodic_tree(root.left)
            self.tree_li.append(root.val)
            self.ergodic_tree(root.right)

    def findTarget(self, root, k):
        self.ergodic_tree(root)
        # 设置首尾指针
        i, j = 0, len(self.tree_li) - 1
        # 设置是否存在Target标志
        flag = False
        while i < j:
            sum = self.tree_li[i] + self.tree_li[j]
            if sum == k:
                flag = True
                break
            elif sum < k:
                i += 1
            else:
                j -= 1
        return flag
```
