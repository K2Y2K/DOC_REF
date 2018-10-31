C++_record

## 一、C++　VS　C

### 1、概念区别

#### 1、C++

C++ 是一种静态类型的、编译式的、通用的、大小写敏感的、不规则的编程语言，支持过程化编程、面向对象编程和泛型编程。

**注意：**使用静态类型的编程语言是在编译时执行类型检查，而不是在运行时执行类型检查。 

面向对象的四大特性：封装、隐藏、继承、多态。

标准的 C++ 由三个重要部分组成：

- 核心语言，提供了所有构件块，包括变量、数据类型和常量，等等。
- C++ 标准库，提供了大量的函数，用于操作文件、字符串等。
- 标准模板库（STL），提供了大量的方法，用于操作数据结构等。

### 2、语法区别

#### 1、初始动态分配

```
#define InitSize 100 //表长度的初始定义
typedef struct{
	ElemType *data;  //指示动态分配数组的指针
    int MaxSize ,length; //数组的最大容量和当前个数
}SeqList;               //动态分配数组顺序表的类型定义

#c的初始动态分配语句
L.data=(ElemType*)malloc(sizeof(ElemType)*InitSize);
#c++的初始动态分配语句
L.data=new ElemType[InitSize];
```

## 二、problem for running

**Problem :**

**Description:**

**Solution:**

### 1、not declared in this scope

#### 1)、error: 'system' was not declared in this scope

**Problem :**

```
error: ‘system’ was not declared in this scope
  system("pause");
```

**Description:**

**Solution:**

```
#include <cstdlib>
```

#### 2)、error: ‘sort’ was not declared in this scope

**Problem :**

```
error: ‘sort’ was not declared in this scope
  sort(array.begin(), array.end(),greater<int>());
```

**Description:**

**Solution:**

```
#include<algorithm>
```

#### 3)、error: ‘printf’ was not declared in this scope

**Problem :**

```
error: ‘printf’ was not declared in this scope
  printf("%d\n",res);
```

**Description:**

**Solution:**

```
#include<stdio.h>
```



## 三、Applets

1、按任意键退出

```
system(“pause”);
#or
if(getchar())
    exit(0);
```

2、在一行输入任意个数

```
whie(1)
{
if(getchar()=='\n')break;
}
#or
for(i=0;;i++)
{
    scanf("%d",num+i);
    if(getchar()=='\n')break;
}
#or
 while (cin.get() != '\n') {
     cin >> i;
     inputs.push_back(i);
 }
```

