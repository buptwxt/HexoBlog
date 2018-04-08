---
title: C++迭代器
date: 2018-04-02 22:48:32
categories:
- 技术 
tags:
- C++ STL 迭代器
---

# 迭代器
每个容器都有自己的迭代器型别
## 迭代器分类：
1. 输入迭代器
2. 输出迭代器，输出迭代器不具有比较操作
3. 前向迭代器，前向迭代器包括input迭代器全部和output迭代器大部分功能，
4. 双向迭代器 ++  --
5. 随机迭代器
<!--more-->
## 相关辅助函数
vector和string 迭代器临时值递增可能编译不过
advance迭代器能前进后退，随机迭代器使用时效率差。
distance，返回两个迭代器距离，pos1必须能够到达pos2，随机迭代器使用时效率差。
iter_swap 交换两个迭代器所指内容，两个迭代器可以不是同一个容器迭代器

## 迭代器配接器
### 逆向迭代器
rbegin()
rend()
reverse_iterator 
base()
### 安插迭代器
*pos 和 pos等效
赋值动作转化为安插操作
++iter 无效

前插
后插
通用插

back_insert_iterator push_back back_inserter
front_insert_iterator front_back front_inserter
insert_iterator insert inserter





