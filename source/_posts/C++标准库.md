---
title: C++标准库
date: 2018-03-27 22:02:06
categories:
- 技术 
tags:
- C++ STL
---

## 基础

### 模板
非类型模板参数 bitset
缺省模板参数
typename
模板成员函数（包括构造函数，但不能为virtual和缺省值）

### 基本类型显示初始化
int i = int()
基础类型会被初始化为0
<!--more-->

### 异常
void func() throw(); // 不抛出异常
void func() throw(bad_alloc); // 跑出bad_alloc异常
try catch
throw

### 命名空间
### bool 类型
### explicit 和 implicit
### 类型转换
dynamic_cast 属于执行期转换
### 常数静态成员
类中可以初始化
但是仍然需要在类外定义

### 头文件
标准C++头
标准C头
旧C头

### 错误和异常
语言支持的异常
例如: 
    new 失败
    dynamic_cast 失败
C++标准库发出的异常
    派生自logic_error
程序作用域外的异常
    runtime_error
    
图：
- exception 
  -- bad_alloc
  -- bad_cast
  -- bad_typeid
  -- logic_error
  ---- domain_error
  ---- invalid_argument
  ---- length_error
  ---- out_of_range
  -- ios_base::failure
  -- runtime_error
  ---- range_error
  ---- overflow_error
  ---- underflow_error
  -- bad_exception   

异常类别成员函数
what() 返回异常信息

派生新类别可以重写 what() 虚函数
配置器默认就好了

### pairs
直接构造
make_pair()函数构造

### auto_ptr
获取资源
执行操作
释放资源
比try catch 更加优雅

auto_ptr可以拿来当做另外一个auto_ptr的初值，普通指针不行

所有权转移
函数是数据终点
函数是数据起点

const auto_ptr<int> p(new int)
const 意味着不能更改auto_ptr的所有权，可以修改执行的内容
const auto_ptr 作为参数，新对象的任何赋值会导致编译错误

auto_ptr 作为成员，当对象删除时，auto_ptr会自动删除其所成员对象。为了避免意外转移所有权，可以将成员设置为const auto_ptr

易错点：
auto_ptr 不能共享所有权
不存在指向数组的auto_ptr（不是delete []）
不属于引用计数型智能指针
不满足STL容器对元素要求（拷贝和赋值后，原auto_ptr和新auto_ptr不等）

### 数值极限
climits 和 cfloat头文件
numeric_limits 类模板
有一些成员函数

### 辅助函数
min
max
swap

### 辅助性比较操作符号
using namespace std::rel_ops中定义了 != >= <= >
基于 == 和 <

### cstddef和cstdlib头文件
NULL 
size_t
ptrdiff_t
offsetof

atexit
exit
_exit
abort
EXIT_SUCCESS
EXIT_FAILURE

## STL 库

### 顺序容器
Vector
Lists
Deque

string 看做特殊的vector
array 关键看与算法的配合

### 关联式容器
Maps可以看做是关联式数组
Sets看做是特殊的Maps
可以设置比较算法，默认<
所有关联式容器都有insert方法，没有push_back和push_front，毕竟没权利指定位置

### 容器适配器
stack
Queues
Priority queue 下一个元素永远是优先级最高的元素

### 迭代器
迭代器后置递增没有前置递增效率高，需要一个额外临时对象

迭代器是一个与对应容器关联的组件
双向存取迭代器
随机存取迭代器

### 算法
独立于数据结构
与迭代器相关
泛型函数式编程思想
算法区间有效
操作多区间算法，注意区间是否有效
equal
copy
可以一开始就给定容器大小，或者resize显示改变容器大小

### 迭代器适配器
插入型
流型
反转型

插入型
back_inserter push_back函数 vector list deque
front_inserter push_front函数 list deque
iserter 可以应用于关联式容器，需要指定插入位置，作为提示

流迭代器
istream_iterator<string>(cin) cin >> string
istream_iterator<string>()
ostream_iterator<string>(cout)

反转迭代器
rbegin
rend

### 变更算法
remove 返回新的结束点位置
distance
容器调用erase
为什么算法不直接erase?
数据结构和算法分离。迭代器只不过是容器某一位置的抽象概念，迭代器对自己所属于的容器一无所知。任何以迭代器访问容器的算法，都无法通过迭代器调用容器类别提供的任何成员函数
关联式容器删除元素不能使用更改型算法

### 成员函数vs算法
尽量用成员函数，算法可能效率低一些
remove算法没有真的删除元素
list的remove算法删除了元素

### 自定义泛型函数
```
template <class T>
inline void PRINT(const T&coll){
    typename T::const_iterator pos;
    for(pos = coll.begin(); pos != coll.end(); pos++){
        cout << *pos << " ";
    }
    cout << endl;
}
```

### 函数作为算法参数
指定搜寻准则、排序准则、或者定义某种操作
例如：
for_each()
transform（）

#### 判断式

面对同样的输入得出同样的结果，排除具有记忆功能的函数
一元判断式
find_if()
二元判断式
sort()

### 仿函数
传递给算法的“函数型参数”，并不一定得是函数，可以使行为类似函数的对象，叫做仿函数
定义了operator ()函数
```
Class X{};
X x;
x(); <===> x.operator()();
```
仿函数可以作为算法的参数
```
class PrintInt{
    public:
    operator()( int i){
        cout << i << endl;
    }
};
for_each( ivec.begin(), ivec.end(), PrintInt() );
```
仿函数优点：
1. 属于智能函数，有成员变量和成员函数；可以在执行期初始化仿函数
2. 有自己的型别（？）
3. 比一般函数执行速度快

预定义仿函数
less<int>
greater<int>
negate<int>
multiplies<int>
bind2nd(multiplies<int>(), 10)
mem_func_ref(&Person::save)

### STL错误和异常
STL错误检查降低效率

1. 迭代器必须合法有效，防止操作过程让迭代器无效
2. 逾尾迭代器operator * ->操作
3. 区间必须合法
4. 多区间操作，必须确保区间均有效
5. 覆盖操作必须确保目标区间空间足够，否则用插入迭代器


很难确定应该为STL库提供怎样的安全性
处理异常本身带来负面影响

保证不发生资源泄露
不与容器的恒常特性发生抵触

事务安全
以节点为基础的容器（list set map等），节点构造失败，
保持容器不变。删除操作，保证不变。关联插入多个，为了保证有序，做不到失败回滚
list 除了remove remove_if merge sort unique,都是事物安全的

以array为基础的vector和deque，插入失败，不能做到完全恢复。push pop能够保证事物安全

swap 可以使 array为基础的容器事物安全

## 容器
### 容器共同能力
容器初始化、获取大小函数、比较函数、赋值操作
可以以B类型容器初始化A类型容器
赋值效率没有swap高

### Vector
动态数组
多了capacity成员函数
调整容量费时，原来的引用 指针 迭代器会失效
减小容器容量窍门 swap

构造拷贝析够
非变动操作
赋值操作
所有赋值操作会调用元素构造 析够等函数，视元素数量变化而定
元素存取，返回引用，注意确保函数存在
迭代器相关函数
插入和删除操作，注意迭代器合法性
vector当一般数组使用注意事项

#### vector异常处理
push_back异常，则不起作用
如果copy操作不抛出异常，则insert要么成功，要么不生效. erase clear不抛出异常
pop_back 不抛出异常
swap不抛出异常
析够不抛出异常

#### vector<bool>
动态bitset
特殊的vector
reference包裹
[] at front back操作






