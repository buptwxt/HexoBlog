---
title: cJSON库介绍
date: 2017-09-27 15:32:00
categories:
- 技术
tags:
- JSON
---

## 正文
JSON作为一种轻量级的数据交换格式，易于学习和使用，广泛应用于当前计算机应用中。计算机应用中会频繁的解析JSON格式数据内容，和生成JSON格式字符串，所以有必要生成一套库，简化操作。cJSON是一套解析JSON格式和生成JSON格式串的库，供c/c++操作，支持跨平台，代码行数小。本文旨在帮助理解该库，能够轻松的将该库应用到自己的项目中。
```
关键词/检索词：c, JSON，接口，数据交换, 跨平台
```

##  概述
### JSON介绍
JSON像XML，但是更苗条，是一个轻量级数据交换格式。JSON格式数据对于人来说，便于读写；对于机器来说，便于解析和生成。JSON是一个完全独立于语言的文本格式，但使用更加方便，使用了与C家族语言程序员所熟悉的格式，这使得JSON成为了一个理想的数据交换语言。可以使用JSON格式传输数据，存储和表示程序状态。
###   cJSON介绍
cJSON旨在成为一款最简单JSON格式数据解析器，便于完成工作。它由单个c源文件cJSON.c和头文件 cJSON.h组成。还包括一个测试文件test.c。在自己的项目中导入cJSON.h头文件和cJSON.c源文件，能够轻松的解析JSON格式数据和生成JSON格式数据。
###  安装使用 
 验证可行性
测试代码是否可用，编译出可执行文件test， 由于cJSON.c依赖数学库libm.so，所以编译方式如下：
编译出可执行文件test
gcc cJSON.c test.c -o test -lm 
执行可执行文件
./test
可以修改test.c文件main函数，控制测试代码输入、输出数据
生成可执行程序，执行可执行程序查看到期望输出，说明代码完整
###  项目集成
当项目中需要涉及到解析JSON格式数据或者生成JSON格式数据时，有两种使用方式，第一、将cJSON.c和cJSON.h放到项目源码文件夹中；第二、将cJSON.c和cJSON.h放到一个单独的文件家中，并添加makefile文件，以库的形式供其函数调用。
需要使用cJSON提供的功能时，建议采用方式一，直接引入cJSON.c和cJSON.h源文件到工程源码中。

## 数据结构
cJSON 节点结构是cJSON库中重要的一个元素，cJSON库将JSON格式字符串数据转化为由一个个cJSON节点表示的树形结构，next指向同级目录的下一个节点，prev指向同级目录前一个节点，child指向下级目录第一个节点；也可以将由cJSON树形结构，转化为JSON格式串，便于在网络中传输JSON格式数据。
cJSON节点数据结构

```typedef struct cJSON {
      struct cJSON *next,*prev;
      struct cJSON *child;
      int type; 
      char *valuestring; 
      int valueint; 
      double valuedouble; 
      char *string;
  } cJSON;```
  
  
一个cJSON节点存储一个JSON格式key:value对，其中
key存储在string中
value存储在valuestring或者valueint、valuedouble中。
type 存储数据类型，包括: "False、True、NULL、Number、String、Array、Object"
child 存储子节点，只有Array和Object类型才有子节点，根节点是一个Object类型
next存储后一个节点
prev存储前一个节点

例如：
JSON格式字符串如下：
```
{
	  "name":"Jack (\"Bee\") Nimble",
	  "format":
	  {
		    "type"：“rect”,
		    "width": 1920,
		    "height": 1080,
		    "interlace": false,
		    "frame rate": 24 
	  }
}
```

转化为对应的cJSON树形结构为：

*  <-  代表prev
*  ->  代表next
*  |    代表child
```
NULL<-root-> NULL
       |
NULL<-name< -----------------> format->NULL
       |                           |
      NULL                NULL  <- type <-> width <->  height <-> interlace <-> framerate->NULL
                                    |           |           |          |          |
                                  NULL       NULL       NULL        NULL          NULL
```
cJSON能够在两种模式下使用，Auto和Manual，建议使用Auto模式，Auto模式下能够实现字符串到cJSON格式直接转化，转化后再获取对应key的value值。

##   常用接口说明
接口参数参考cJSON.h头文件
cJSON_Parse： 字符串转化为cJSON树型结构
cJSON_GetObjectItem：获取key对应的cJSON对象
cJSON_Print：将cJSON树对象转化为JSON格式字符串
cJSON_Delete：清除cJSON树型结构对象
cJSON_CreateObject：创建一个cJSON节点
AddItemToObject：添加Object对象到cJSON树结构
AddNumberToObject：添加value为整型格式cJSON节点
AddFalseToObject：添加value为Bool格式cJSON节点

##  示例
###  解析JSON格式数据
1. 转变为cJSON结构
char *my_json_string 指向一个字符串，该字符串内容为上面的JSON格式数据，获取cJSON格式数据：
```cJSON *root = cJSON_Parse(my_json_string);```
2. 获取"frame rate"数据，并存储于framerate
```cJSON *format = cJSON_GetObjectItem(root, "format");```
```int frame = cJSON_GetObjectItem(format, "frame rate")->valueint;```
3. 修改“frame rate”值为25
```cJSON_GetObjectItem(format, "frame rate")->valueint = 25;```
4. 将cJSON格式再转换为string
```char *rendered = cJSON_Print(root);```
5. 清除资源
```cJSON_Delete(root);```
 注意：在解引用数据时应该判断指针是否合法
 
###   生成JSON格式数据
```
cJSON *root,*fmt; 
root=cJSON_CreateObject();
cJSON_AddItemToObject(root, "name", cJSON_CreateString("Jack (\"Bee\") Nimble"));
cJSON_AddItemToObject(root, "format", fmt=cJSON_CreateObject());
cJSON_AddStringToObject(fmt,"type", "rect");
cJSON_AddNumberToObject(fmt,"width", 1920);
cJSON_AddNumberToObject(fmt,"height", 1080);
cJSON_AddFalseToObject (fmt,"interlace");
cJSON_AddNumberToObject(fmt,"frame rate",   24);
char *pBuf = cJSON_Print(root); // 转化为字符串
```
pBuf指向的字符串可以用于网络传输

## 参考文献
[JSON官方介绍](http://www.json.org )
[cJSON 源码来源](https://www.github.com)