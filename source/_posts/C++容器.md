---
title: C++ 容器
date: 2018-03-30 22:47:29
categories:
- 技术 
tags:
- C++ STL
---

## 容器
### deque
deque 内部数据结构实现
与vector对比起来学习，能够高效在首部添加或者删除成员

### list
双向链表管理元素
不支持随机存取，任何位置插入删除高效，插入和删除不会导致其他元素迭代器、引用、指针失效
要么操作成功，要么什么都不发生
不提供容量空间重新分配操作函数
提供了不少专门用于元素移动的成员函数
remove
remove_if

#### splice函数
unique
splice
sort
merge
reverse

list基本上所有操作都是不成功或者成功，中间状态也不会有资源泄露
只要不调用sort或者赋值操作，并保证元素比较时不抛出异常，那么可以说list是“事物安全”

### sets和multisets
拷贝 赋值 比较的类型都可以作为set的元素
排序准则满足：
1. 反对称
2. 可传递
3. 非自反

不能直接改变元素值，必须先删除旧元素，再插入新元素

1. 只需要传递一个参数作为排序准则
2. 不必针对元素型别提供operator==
3. 可以对相等性有截然相反的定义

比较动作只能用于相同的容器，元素类型和排序准则都必须相同

set搜寻操作函数
count(elem) 返回值为elem元素个数
find(elem) 返回值为elem第一个元素
lower_bound(elem) 返回元素值大于等于elem的第一个元素
upper_bound(elem) 返回值大于elem第一个元素
equal_range(elem) 返回元素值==elem的元素区间

两个排序准则不同，但是元素类型相同的set赋值（或者swap）时，排序准则也会被修改

set容器的迭代器为双向迭代器
remove算法是参数值覆盖被移除的元素，set元素不能被修改，因此不能使用remove算法
insert(pos, elem) pos并不代表真正的位置，只是一种提示，提示的恰当，能加快速率

set insert(elem) 返回pair，位置和是否插入成功
insert 提示得当，复杂度有logn变为分摊常数

erase 删除所有
erase(pos) 删除指定
erase返回void，顺序容器erase返回迭代器

删除总能成功
插入单个要么失败，要么成功；插入多个不能保证要么成功，要么失败

执行期指定排序准则
赋值操作符不仅赋值元素，同时赋值了排序准则
 ```
 template <class T>
class RuntimeCmp {
public:
    enum cmp_mode{normal, reverse};
    
private:
    cmp_mode mode;
public:
    RuntimeCmp(cmp_mode m=normal):mode(m){
        
    }
    bool operator()(const T&t1, const T&t2) const{
        return mode == normal ? t1 < t2 : t2 < t1;
    }
    bool operator==(const RuntimeCmp &rc){
        return mode==rc.mode;
    }
};

typedef set<int, RuntimeCmp<int> > IntSet;
void fill(IntSet &set);
void fill(IntSet &set){
    set.insert(4);
    set.insert(7);
    set.insert(5);
    set.insert(1);
    set.insert(6);
    set.insert(2);
    set.insert(5);
}

void PrintSet(IntSet &set){
    IntSet::iterator iter = set.begin();
    for (; iter != set.end(); ++iter) {
        cout << *iter << " ";
    }
    cout << endl;
}

int main(int argc, const char * argv[]) {
    // insert code here...
    
    IntSet coll1;
    fill(coll1);
    PrintSet(coll1);
    
    RuntimeCmp<int> reverse_order(RuntimeCmp<int>::reverse);
    IntSet coll2(reverse_order);
    //fill(coll2);
    PrintSet(coll2);
    
    set<int> set1;
    set<int> set2( greater<int> );
    
    if( coll1.value_comp() == coll2.value_comp() ){
        cout << "equal" << endl;
    }else{
        cout << "not equal" << endl;
    }
    
    std::cout << "Hello, World!\n";
    
    sleep(10);
    
    return 0;
}
 ```
 
### maps和multimaps
构造函数
1. 以模板参数定义
2. 以构造函数参数定义

与set类似，set可以看做map的特例
map的key为const

value_type
pair
make_pair
三种方式将value传入map

遍历删除元素
```
for ( SIMap::iterator iter = myMap.begin(); iter != myMap.end(); ) {
        if (iter->second == 28) {
            myMap.erase(iter++);
        }else{
            ++iter;
        }
    }
```

注意map重载了operator[]，返回引用；元素不存在则，则加入该元素，防止误加入新元素

find_if 算法查询或者遍历查询value为某一个值得元素

cout.setf(ios::left, ios::adjustfield);

### 容器其他
deque元素被删除时，容器能够自动缩减内存










