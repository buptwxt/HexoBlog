---
title: 需要熟练使用的Linux命令
date: 2018-07-29 09:23:36
categories: 
- 技术 
tags:
- Linux常用命令
---

# 文本处理相关命令
## sort
-u 仅仅忽略连续相同的行
-n 数字
-r 逆序
-k 比较域
-t 分割符号
-s 稳定，以最后的排序规则稳定
```
sort -k1 file.txt | sort -s k2 file.txt
```
一定是file的field 2稳定不变后再排序field 1
<!--more-->

## uniq
配合sort使用
-u 输出不重复行
-d 输出重复行
-c 统计次数

## cut
-c 指定选取的字符
-f 指定选取的域
-d 分隔符
--complement 反选
n1-n2 选取范围从n1到n2
```
cut -f1-3 -d" " file.txt
```

## paste
- 格式制定
-d 分割符号
-s 转置
cat b | paste - -

## join
-j 1 制定域作为分割
-1 2 以1文件FIELD 2作为匹配
-2 3 以2文件FIELD 3作为匹配
-o 1.1 输出1文件1列
-v 1 仅仅显示没有匹配上的文件1的信息
-v 1 显示匹配上的和没有匹配上的文件1的信息

```
join file1 file2 | join - file3 | join - file4
```
```
cat a b | sort | uniq > c
cat a b | sort | uniq -d > c
cat a b b | sort | uniq -u > c
```
## shuf
随机输出文件中的文本行

## strings
-a 扫描整个文件
-f 显示文件名

## iconv

## split
-d 数字后缀
-b 块大小
-l 列

csplit
{*} 分割次数
-s 不打印输出
-n 指定后缀数字个数
-f 指定文件名前缀
-b 指定后缀格式
```
csplit server.log /SERVER/ -n2 -s {*} -f server -b "%02d.log"; rm server00.log
```






