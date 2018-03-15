---
title: vim技巧
date: 2016-09-28 12:00:00
categories:
- 技术
tags:
- CentOS vim
---


## 正文
[《Vim实用技巧》](https://book.douban.com/subject/25869486/)这本书中提到了好多vim下神技，这些技能大量的依赖ctrl按键与其他按键的组合，所以有必要在一个顺手的位置方便操作ctrl按键。大小写按键并不常用，而且占据了一个有利的位置，所以建议vim用户将ctrl按键和大小写切换按键互换。

```
!
!Swap Caps_Lock and Control_L
!
remove Lock = Caps_Lock
remove Control = Control_L
keysym Control_L = Caps_Lock
keysym Caps_Lock = Control_L
add Lock = Caps_Lock
add Control = Control_L
```
将这段代码保存为文件.Xmodmap，放置在任何目录下，
在终端再执行下面这条命令。

```
xmodmap .Xmodmap
```
