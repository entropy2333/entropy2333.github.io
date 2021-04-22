---
title: C++ Primer Chapter 3 字符串、向量和数组
date: 2020-11-23 16:48:44
tags:
- C++
category:
- C++ Primer

---

介绍标准库string和vector，以及迭代器和数组的使用。

<!--more-->

### 命名空间的using声明

```c++
using namespace::name;
```

头文件不应该包含using声明。

## 标准库类型string

string定义在命名空间std中。

```
#include <string>
using namespace std::string;
```

### 定义和初始化

如果使用等号，实际上执行的是拷贝初始化。反之，执行的是直接初始化。

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130133107639.png" alt="image-20201130133107639" style="zoom: 50%;" />

### string对象上的操作

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130125844111.png" alt="image-20201130125844111" style="zoom:50%;" />

#### 读写

执行读取操作时，会自动忽略开头的空白字符，直至遇到下一处空白。

输入"	Hello World!	"，输出则为"Hello"。

```c++
int main()
{
    string s;
    cin >> s;
    cout << s << endl;
    return 0;
}
```

可以用getline读取一整行。

```c++
int main()
{
    string line;
    while (getline(cin, line))
        cout << line << endl;
    return 0;
}
```

#### empty&size

empty判断string对象是否为空，返回bool。

```c++
while (getline(cin, line))
    if (!line.empty())
        cout << line << endl;
```

size返回string对象的长度，类型为string::size_type。

```c++
while (getline(cin, line))
    if (!line.empty())
        cout << line.size() << endl;
```

> 此时，空字符也会计数。

当使用size_type进行比较时，注意不要与int混用，因为负值的带符号数会转化为大整数的无符号数。

#### 比较&加法

string对象逐字符比较，按字典序比较。

```c++
string s1 = "abc";
string s2 = "abc def";
string s3 = "abdef";
// res: s1 < s2 < s3
```

两个string对象可以相加，字面值也可以和string对象相加。

```c++
string s1 = "hello", s2 = "world"。
string s3 = s1 + s2。
string s4 = s1 + " " + s2 + "\n";
string s5 = "hello" + " " + s2; // error
```

> 运算时，应保证+两侧的运算对象至少有一个是string。

### 处理string中的字符

下表给出了一些关于字符属性的函数。

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130125720977.png" alt="image-20201130125720977" style="zoom:50%;" />

> C++标准库为了兼容C，将C的头文件如name.h，命名为cname。
>
> C++程序应该使用cname形式的头文件，以区分从C继承的头文件。

为了遍历string对象，可以使用下标，也可以使用范围for语句（推荐）。

```c++
string s = "hello world";
for (auto c : s)
    cout << c << endl;
```

如果想要改变字符，需要把循环变量定义为引用类型。

```c++
string s = "hello world";
for (auto &c : s)
    c = toupper(c);
cout << s;
```

有时候只需要处理部分字符，可以使用下标，也可以使用迭代器（后续介绍）。

```c++
// 实现单词首字母大写
string s = "hello world";
for (decltype(s.size()) index = 0;
     index != s.size(); ++index) {
    if (index == 0 && isalpha(s[index])) {
        s[index] = toupper(s[index]);
        continue;
    }
    else if (isalpha(s[index]) && !isalpha(s[index-1]))
        s[index] = toupper(s[index]);
}
cout << s; // output: "Hello world"
```

> 使用下标时，切忌下标越界，应该时刻检查下标的合法性。

## 标准库类型vector

vector表示相同类型对象的集合，每个对象都有一个索引用于访问对象。因此vector也被称为容器，vector同时也是一个类模板（距离还比较遥远）。

```c++
#include <vector>
using std::vector;
```

模板实例化时，需要提供对象类型。

```c++
vector<int> ivec;				// 元素是int
vector<vector<string>> file;	// 元素是存放string的vector
```

vector作为类模板，不是类型。

引用不是对象，不存在包含引用的vector。

### 定义和初始化vector对象

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130133133358.png" alt="image-20201130133133358" style="zoom:50%;" />

注意区分列表初始化和元素数量。

```c++
vector<int> v1{10}; // 列表初始化，v1有1个元素，值为10。
vector<int> v2(10); // v2有10个元素，值为0。

vector<int> v3{10, 1}; // 列表初始化，v3有2个元素，值为10和1。
vector<int> v4(10, 1); // v4有10个元素，值为1。
```

### 向vector对象添加元素

可以用vector的成员函数push_back添加元素。

```c++
vector<int> v;
for (int i = 0; i != 10; ++i)
    v.push_back(i);
```

> 与C不同，C++推荐先定义一个空的vector对象，运行时向其中添加具体值。

### 其他vector操作

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130133935086.png" alt="image-20201130133935086" style="zoom:50%;" />

vector同样支持范围for语句。

```c++
vector<int> v = {1, 2, 3, 4, 5};
for (auto &i : v)	// 使用引用对vector的元素赋值
    i *= 2;
for (auto i : v)
    cout << i << " ";
```

与string类似，vector的size函数返回类型为vector<T>::size_type。

```c++
// 统计各分数段内的人数
// input: 42 65 95 100 39 67 95 76 88 76 83 92 76 93 101
// output: 0 0 0 1 1 0 2 3 2 4 1
vector<unsigned> scores(11, 0);
unsigned grade = 0;
while (cin >> grade) {
    if (grade <= 100)
        ++scores[grade/10];
    else break;
}
for (auto i : scores)
    cout << i << " ";
```

## 迭代器介绍

### 使用迭代器

有迭代器的类型拥有名为begin和end的成员。

begin成员返回指向第一个元素的迭代器，end成员返回指向容器“尾后”元素的迭代器，通常被称作尾后迭代器。

如果容器为空，begin和end返回的是同一个迭代器，都是尾后迭代器。

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130135012221.png" alt="image-20201130135012221" style="zoom:50%;" />

```c++
string s = "hello world";
for (auto it = s.begin(); it != s.end(); ++it)
    if (isalpha(*it))
        *it = toupper(*it);
```

> C++习惯在for循环中使用!=进行判断，因为大多数迭代器都没有定义<运算符。

迭代器也有const类型，能读取但不能修改vector。

为了便捷，C++11引入了两个新函数cbegin和cend。

```c++
vector<int> v1;
const vector<int> v2;
auto it1 = v1.begin();	// type: vector<int>::iterator
auto it2 = v2.begin();  // type: vector<int>::const_iterator
auto it3 = v1.cbegin(); // type: vector<int>::const_iterator
```

为了简化解引用和成员访问操作一起使用，C++定义了箭头运算符。

```c++
*it.empty;   // error
(*it).empty;
it->empty;
```

### 迭代器运算

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130135031052.png" alt="image-20201130135031052" style="zoom:50%;" />

```c++
// 得到最接近中间元素的迭代器
auto mid = v.begin() + v.size() / 2;
```

C++定义了迭代器的差值，类型为difference_type。

```c++
// 二分搜索
vector<string> text;
string sought;
auto beg = text.begin(), end = text.end();
auto mid = text.begin() + (end - beg) / 2;
while (mid != end && *mid != sought) {
    if (sought < *mid) // 在前半部分则忽略后半部分
        end = mid;
    else
        beg = mid + 1; // 否则忽略前半部分
    mid = beg + (end - beg) /2;
}
```

## 数组

与vector不同，数组的大小确定不变，不能随意增加元素。

### 定义和初始化

数组的长度在编译时必须已知，所以维度必须是常量表达式。

定义数组时，必须指定类型，不能使用auto。和vector一样，不存在引用的数组。

```c++
unsigned cnt = 42;
constexpr unsigned sz = 42;
int arr[10];
int *parr[sz];	 // 包含42个整型指针的数组
string bad[cnt]; // error
```

数组也支持列表初始化，此时可以不指明长度，编译器会自动计算。

```c++
const unsigned sz = 3;
int ial[sz] = {0, 1, 2};
int a2[] = {0, 1, 2};
int a3[5] = {0, 1, 2};	// a3[] = {0, 1, 2, 0, 0}
```

值得注意的是，在使用字符数组时，会在结尾添加一个空字符。

```c++
char a1[] = {'C', '+', '+'};
char a2[] = {'C', '+', '+', '\0'};	// 显式添加空字符
char a3[] = "C++";	// 自动添加空字符
char a4[6] = "Daniel"; // error
```

上述表达式中，a1长度为3，a2和a3的长度都是4，a4长度应改为7。

数组不允许拷贝和赋值。

```c++
int a[] = {0, 1, 2};
int a2[] = a; // error
a2 = a; // error
```

为了理解复杂的数组声明，可以从内向外阅读。

```c++
int *ptrs[10]; // 含有10个整型指针的数组
int &refs[10]; // 不存在引用的数组
int (*Parray)[10] = &arr; // Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arr;  // arrRef引用一个含有10个整数的数组
int *(&arry)[10] = ptrs;  // arry是数组的引用，该数组含有10个指针
```

### 访问数组元素

数组也可以用范围for语句或下标运算符访问。

数组下标被定义为size_t类型，定义在cstddef头文件中。

### 指针和数组

使用数组的时候，编译器一般会把它转换成指针。

```c++
string nums[] = {"one", "two", "three"};
string *p1 = &nums[0]; // p1指向nums的第一个元素
string *p2 = &nums;	   // p2和p1等价
```

尽管能计算得到数组的尾后指针，但容易出错。C++11引入两个函数begin和end，与容器的同名成员功能类似。

```c++
int ia[] = {0, 1, 2, 3, 4, 5};
int *beg = begin(ia);
int *last = end(ia);
```

与容器相似，指针相减的类型名为ptrdiff_t，也定义在cstddef中。

#### C风格字符串

> 尽管C++支持，但最好不好使用。

<img src="C++ Primer Chapter 3 字符串、向量和数组/image-20201130144349734.png" alt="image-20201130144349734" style="zoom:50%;" />

#### 与旧代码的接口

允许使用字符串字面值初始化string对象，反过来则不行，需要使用c_str函数。

```C++
string s("hello world");
char *str = s; // error
const char *str = s.c_str();
```

可以使用数组初始化vector对象

```c++
int int_arr[] = {0, 1, 2, 3, 4, 5};
vector<int> ivec(begin(int_arr), end(int_arr));
```

> C++应当尽量使用vector和迭代器，而非内置数组和指针；应当尽量使用string，而非C风格字符串。

## 多维数组

多维数组实际上就是数组的数组。

```c++
int ia1[3][4] = {
    {0, 1, 2, 3},
    {4, 5, 6, 7},
    {8, 9, 10, 11}
};
int ia2[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};
int ia3[3][4] = {{ 0 }, { 1 }, { 2 }}; // 初始化每行的第一个元素
int ia4[3][4] = {0, 3, 6, 9}; // 初始化第一行
```

可以通过下标运算符访问多维数组的元素。

```c++
constexpr size_t rowCnt = 3, colCnt = 4;
int ia[rowCnt][colCnt];
for (size_t i = 0; i != rowCnt; ++i) {
    for (size_t j = 0; j != colCnt; ++j) {
        ia[i][j] = i * colCnt + j;
    }
}
```

也可以用范围for语句访问，实现同样的效果。

```c++
size_t cnt = 0;
for (auto &row : ia) {
    for (auto &col : row) {
        col = cnt;
        ++cnt;
    }
}
```

因为循环中要赋值，所以选用引用类型。其实除了内层循环，其他所有循环的控制变量都应该是引用类型。

```c++
for (auto row : ia)
    for (auto col : row)
```

上面这段语句将无法通过编译，因为row不是引用类型，所以row的类型是int*。

```c++
int *ip[4];   // 整形指针的数组
int (*ip)[4]; // 指向含有4个整数的数组
```

