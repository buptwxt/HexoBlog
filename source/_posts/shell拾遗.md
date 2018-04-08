---
title: shell拾遗
date: 2018-04-06 13:56:42
- 技术 
tags:
- Linux Shell
---

& && 区别

[ condition ]

###echo 获取字符串
echo ${new:-variable}
echo {variable:x:y}
variable 代表shell变量
x: 起始位置
y: 截取长度

vari="User:123:321:/home/dir"
echo ${vari##*:}
echo ${vari%%:*}


### 获取变量长度
${#vari}

###${variable:-10} 和 ${variable: -10} 有什么区别?

### set
取消变量和取消变量赋值

### expr
算数运算
或者$、[]计算


### tr

${vari} 与 $vari 区别

==
-eq
-le
-gt

[[ $vari == abc* ]]

### $! 含义
read
expect


### 获取swap大小
top
free
swapon -s

### lastlog
who w

sed
awk

### 统计词频

例如：第三个词汇排名前10
awk -F "" '{print $3}' input.txt |
sort | uniq -c | sort -r | head -10

### 合并文件并排序
join f1 f2 | sort -k 2

### sed

### awk

ARGIND 指示输入文件
```
cat name.txt
```
wang
li
zhang

```
cat score.txt
```
wang 100
li 100
zhang 99
guo 98
zhao 97

```
awk -F" " 'ARGIND==1{grade[$1]=$0} ARGIND>1 && ($i in grade){print grade[$1]}' score.txt name.txt  
```
wang 100
li 100
zhang 99


### grep
统计两个文件相同的行
grep -Ff f1 f2

统计f1有，f2没有
grep -vFf f2 f1

### iotop
查看IO状况



