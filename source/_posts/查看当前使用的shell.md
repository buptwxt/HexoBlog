---
title: 查看当前使用的shell
date: 2018-04-21 21:23:06
categories:
- 技术 
tags:
- Linux Shell
---

## 查看当前发行版可以使用的shell
[jack@localhost ~]$ `cat /etc/shells `
/bin/sh
/bin/bash
/sbin/nologin

<!--more-->
验证环境为macOS High Sierra
version: 10.13.3
## 查看当前使用的shell
1. 最常用的查看shell的命令，但不能实时反映当前shell
user@wxt: ~ $ `echo $SHELL `
/bin/bash

2. 并不是所有shell都支持
user@wxt: ~ $ echo $0
-bash

3. 环境变量中shell的匹配查找
user@wxt: ~ $ env | grep SHELL
SHELL=/bin/bash


4. 口令文件中shell的匹配查找
    centos 可以查看
user@wxt: ~ $ `cat /etc/passwd | grep user`

5. 查看当前进程
user@wxt: ~ $ `ps`
PID TTY           TIME CMD
 2616 ttys000    0:00.14 -bash
 2964 ttys000    0:00.13 bash
 3200 ttys001    0:00.07 -bash

6. 先查看当前shell的pid，再定位到此shell进程
user@wxt: ~ $ `echo $$`
3200
user@wxt: ~ $ `ps -ef | grep 3200`
502  3200  3199   0  9:43下午 ttys001    0:00.06 -bash
    0  3216  3200   0  9:46下午 ttys001    0:00.01 ps -ef
  502  3217  3200   0  9:46下午 ttys001    0:00.00 grep 3200

附：一条命令即可实现：
user@wxt: ~ $ `ps -ef | grep `echo $$` | grep -v grep | grep -v ps`
502  3200  3199   0  9:43下午 ttys001    0:00.04 -bash


7. 输入一条不存的命令，查看出错的shell提示
user@wxt: ~ $ `wang`
-bash: wang: command not found

原文链接 <https://www.cnblogs.com/zfc2201/articles/6032292.html>


