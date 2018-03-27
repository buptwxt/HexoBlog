---
title: Go语言编程学习笔记 2
date: 2018-03-26 21:24:42
categories:
- 技术 
tags:
- GO
---

# 面向对象
Go不具有继承、虚函数、构造函数、析构函数、隐藏this指针等传统面向对象编程特征。
## 类型系统
类型系统指一门语言的类型体系结构。典型的类型系统包括：
* 基础类型
* 复合类型
* 可以指向任意对象的类型
* 值语义和引用语义
* 面向对象，即所有具备面向对象特征的类型
* 接口
类型系统描述了类型体系结构如何被关联
Go语言中大多数类型都是值语义，并且可以包含对应的操作方法。在需要的时候，可以给任何类型“增加”新方法。实现某个接口时，无需从该接口继承，只要实现该接口要求的所有方法即可。任何类型都可以被Any类型引用。Any类型是空接口，即interface{}

### 为类型添加方法
```
type Interger int

func (a *Interger)Add(b Interger){
    *a += b
}
```
注意需要修改变量的值时，传指针
“Go语言中没有隐藏this指针”
施加的目标显示传递，没有被隐藏
不需要非得是指针，也不用非得叫this

### 值和引用语义
基本类型和符合类型都是基于值语义
byte bool float32 string等
数组 结构体 指针等

a, b不同
```
var a = [3]int{1,2,3}
var b = a
b[1]++
```

想表达引用，需要用指针
a, b相同
```
var a = [3]int{1,2,3}
var b = &a
b[1]++
```

Q: 数组和对数组取地址区别？

4个看起来像引用类型，他们本质上与指针相关
* 数组切片
* map
* channel
* 接口

### 结构体
放弃了大量面向对象特征，保留了组合特征
跟C结构体非常像
``` 
type Rect struct{
    x, y float64
    width, height float64
}

func (r *Rect) Area() float64{
    return r.width * r.height
}

var r Rect
r.width = 10
r.height = 10
fmt.Println( r.Area() ) 
```

