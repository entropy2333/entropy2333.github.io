---
title: C++ Primer Chapter 1 开始 
date: 2020-11-23 14:29:28
tags:
- C++
category:
- C++ Primer
---

初识C++。

<!--more-->

## 编写一个简单的C++程序

每个C++程序都包含若干个函数，其中一个必须命名为main，操作系统通过调用main运行C++程序。

```C++
int main()
{
    return 0;
}
```

### 编译、运行程序

程序运行后，可以通过echo命令访问main的返回值。

```bash
echo $? // UNIX
echo $ERRORLEVEL% // Windows
```

## 初识输入输出

| 对象 | 用途                   |
| ---- | ---------------------- |
| cin  | 标准输入               |
| cout | 标准输出               |
| cerr | 标准错误               |
| clog | 程序运行时的一般性信息 |

### 注释简介

```C++
// 单行注释
/*
	多行注释
	不能嵌套
/*
```

## 控制流

while & for & if

### 读取数量不定的输入数据

当遇到文件结束符（EOF）或遇到一个无效输入时，istream对象的状态会变成无效，条件为假。

```C++
#include <iostream>
int main()
{
    int sum = 0, value = 0;
    while (std::cin >> value)
        sum += value;
    std::cout << "Sum is " << sum << std::endl;
    return 0;
}
```

> Windows系统中，输入EOF的方式是Ctrl + Z；UNIX中，方法是Ctrl + D。

### 统计输入的词频

```C++
#include <iostream>
int main()
{
    int currVal = 0, val = 0;
    if (std::cin >> currVal) {
        int cnt = 1;
        while (std::cin >> val)
            if (val == currVal)
                ++cnt;
        	else {
                std::cout << currVal << " occurs "
                          << cnt << " times " << std::endl;
                currVal = val;
                cnt = 1;
            }
        std::cout << currVal << " occurs "
                  << cnt << " times " << std::endl;
    }
    return 0;
}
```

## 类简介

> 包含来自标准库的头文件，使用<>包含头文件名；不属于标准库的头文件，使用""包围。