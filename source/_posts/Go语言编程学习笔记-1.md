---
title: Go语言编程学习笔记 1
date: 2018-03-25 22:45:24
categories:
- 技术 
tags:
- GO
---

# Go语言编程学习笔记

## 基础介绍
起源
go语言为静态语言
GOPATH
项目目录结构
go build
go test
go run
go install

### 语言特性
自动垃圾回收
丰富的内置类型
函数多返回值
错误处理
匿名函数和闭包
类型和接口
并发编程
反射
语言交互性

<!--more-->

## 顺序编程
### 变量
申请和初始化
var (
    i int
    j int
)
a := 10
赋值
i, j = j, i
_ (匿名变量)

### 常量
常量是无类型
const
编译期行为
true false iota
iota碰见const就初始为0，自动增长

### 枚举
 不支持enum
 通过const ()实现
 大写字母开头常量，包外可见；小写字母开头常量，包内可见
 
### 丰富类型
####bool 不支持支持或者强制转换
####int和int32不是一个类型，编译器不会自动转换
不同类型整数不能直接比较
^a a取反（~a： C类型取反）
fvalue2 := 12.0 推倒为float64
####浮点型比较方法：
```
// p 为用户定义比较精度，例如：0.00001
import "math"
func IsEqual(f1, f2, p float64) bool{
    return math.Fdim(f1, f2) < p
}
```
####复数
```
complex64
complex(a, b)
a + bi
real() image()
math/cmplx 库
```
####字符串
len()
标准支持 UTF-8 和 Unicode编码
strings 库

rune Unicode unicode包 unicode/utf8
byte UTF8 

####数组
var myarray [10]int = [10]int{1,2,3..}
myarray := [10]int{1,2,3..}
range 数组

####数组切片
类似vector
依据数组创建
依据make() 创建
提前分配好容量
cap() 
len()
append()  ...
new2 = append( new2, newSlice... )
取切片的值范围，只要不超过cap()返回值
copy() //按照较小切片复制

#### map
```
package main
import "fmt"
type PersonInfo struct{
    ID string
    Name string
    Address string
}
func main(){
	var personDB map[string] PersonInfo
    personDB = make(map[string] PersonInfo)

    personDB["12345"] = PersonInfo{"12345", "Tom", "Room"}
    person, ok := personDB["12345"]
    if ok {
        fmt.Println(person)
    }else{
        fmt.Println("not found")
    }
}
```
声明变量
make() 创建
delete() 删除
查找

### 流程控制
选择语句多了select
循环语句少了while多了range

if else if else
注意语法格式，与C有差别
最终返回值不能再if语句中,可以直接return（代表异常退出）

switch 注意语法
fallthrough
case 多个
不会穿透
switch 后可以不跟表达式，而将表达式放在case语句中

for
赋值多个变量
break 可以选择中断哪一个循环

### 函数
小写大写名字区别
不定参数
不定参数传递
任意类型不定参数
```
func MyPrintf(args ... interface{}){
    for _, arg := range args{
        switch arg.(type){
        case int:
            fmt.Println("int")
        case string:
            fmt.Println("string")
        }
    }
}
```
多返回值
匿名函数 与 C回调函数对比
* 闭包
包含自由变量的代码块
闭包可以引用函数外的变量

defer 关键字
程序异常时，被defer修饰的句子被执行，栈调用过程，defer还可以修饰函数
```
defer func(){
    fmt.Println("defer")
}()
```    

panic()
正常的函数执行流程立刻终止，defer修饰的部分正常执行，函数返回到调用函数，并逐层向上执行panic。直到所属goroutine中所有正在执行函数终止。
recover()
终止错误处理流程，常用于defer修饰的函数
#### 常用包
flag包

go 函数 返回值 还需要定义吗？

