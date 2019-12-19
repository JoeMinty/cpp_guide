# 字符串、向量和数组
## 命名空间的using的声明
**`using namespace::name;`**

### 每个名字都需要独立的using声明
按照规定，每个using声明引入命名空间的一个成员
```c++
using std::cin;
using std::cout;
using std::endl;
```

### 头文件不应包含using声明
因为头文件的内容会拷贝到所有引用它的文件中去，如果头文件中有某个using声明，那么每个使用了该头文件的文件就都会有这个声明

## 标准库类型string
标准库类型**string**表示可变长的字符序列，作为标准库的一部分，string定义在命名空间std中
```c++
#include <string>
using std::string;
```

### 定义和初始化string对象
#### 直接初始化和拷贝初始化
如果使用等号（=）初始化一个变量，实际上执行的就是**拷贝初始化（copy initialization）**，编译器把等号右侧的初始值拷贝到新创建的对象中去

如果不使用等号，则执行的是**直接初始化（direct initialization）**

### string对象上的操作
#### 读写string对象
**cin**

#### 读取未知数量的string对象

#### 使用getline读取一整行

#### string的empty和size操作
- **empty**函数根据string对象是否为空返回一个对应的布尔值

- **size**函数返回string对象的长度

#### string::size_type类型
```c++
string line;
getline(cin, line);
auto len = line.size(); // len的类型是string::size_type
```

#### 比较string对象

#### 为string对象赋值
```c++
string str1(10, 'c'), str2;
str1 =  str2;
```

#### 两个string对象相加

#### 字面值和string对象相加
切记字符串字面值并不是标准库类型string的对象，字符串字面值与string是不同的类型

### 处理string对象中的字符
#### 处理每个字符？使用基于范围的for语句
```c++
for (declaration : expression)
  statement
```

#### 使用范围for语句改变字符串中的字符
```c++
string s("Hello World!!!");
for (auto &c : s)
  c = toupper(c);

cout << s << endl; 
```

#### 只处理一部分字符？
**下标运算符[]** 的输入参数是string::sizt_type类型的值，这个参数表示要访问的字符的位置，返回值是该位置上字符的引用

#### 使用下标执行随机访问
无论何时使用字符串的下标，都应该注意检查其合法性。


## 标准库类型vector

```c++
#include <vector>
using std::vector;
```

### 定义和初始化 vector 对象
#### 列表初始化 vector 对象
```c++
vector<string> articles = {"a", "an", "the"};
```

#### 创建指定数量的元素
```c++
vector<int> ivec(10, -1);

vector<string> svec(10, "hi");
```

#### 值初始化

#### 列表初始值还是元素数量?
```c++
vector<int> v1(10); // v1有10个元素，每个的值都是0

vector<int> v2{10}; // v2有一个元素，该元素的值为10

vector<int> v3(10, 1); // v3有是个元素，每个的值都为1

vector<int> v4{10, 1}; // v4有2个元素，值分别为10和1
```
圆括号提供的值是用来构造vector对象

花括号用来表述列表初始化vector对象

### 向 vector 对象中添加元素
vector 的成员函数 `push_back` 向其中添加对象，放入尾部

#### 向 vector 对象添加元素蕴含的编程假定
范围for语句体内不应改变其所遍历序列的大小

### 其他 vector 操作
#### 计算vector内对象的索引
使用下标运算符能获取到指定的元素，vector对象的下标从0开始，下标的类型是相应的`size_type`类型
```c++
vector<int>::size_type; // 正确

vector::size_type; // 错误
```

#### 不能用下标形式添加元素
vector 对象（以及 string 对象）的下标运算符可用于访问已存在的元素，而不能用于添加元素

```c++
#include <iostream>
#include <vector>

using std::cin;
using std::cout;
using std::cerror;
using std::endl;
using std::vector;

int main()
{
  vector<int> ivec;
  int i;
  while (cin >> i)
  {
    ivec.push_bak(i);
  }
  
  for (int i = 0; i < ivec.size() - 1; i++) 
  {
    cout << ivec[i] + ivec[i+1] << endl;
  }
  
  cout << "=======================" << endl;
  
  int left = 0;
  int right = ivec.size() - 1;
  while (left < right) 
  {
    cout << ivec[left] + ivec[right];
    left++;
    right--;
  }
  
  return 0;
}

```

## 迭代器介绍
### 使用迭代器
有迭代器的类型同时拥有返回迭代器的成员

**`begin`** 成员负责返回指向第一个元素（或第一个字符）的迭代器

**`end`** 成员则负责返回指向容器（或string对象）“尾元素的下一位置（pne past the end）”的迭代器，`end`成员返回的迭代器通常被称作为**尾后迭代器**（off-the-end iterator）或简称为尾迭代器（end-iterator）

如果容器为空，则begin和end返回的是同一个迭代器，都是尾后迭代器

```c++
auto b = v.begin(), e = v.end();
```

#### 迭代器运算符
```c++
string s("some thing");
while(s.begin() != s.end())
{
  auto it = s.begin();
  *it = toupper(*it)
}
```

#### 将迭代器从一个元素移动到另外一个元素
```c++
// 依次处理s的字符直至字符串或者遇到空白
for (auto it = s.begin(); it != s.end() && !isspace(*it); ++it)
{
  *it = toupper(*it);
}
```

#### 迭代器类型
拥有迭代器的标准库类型使用iterator和const_iterator来表示迭代器类型
```c++
vector<int>::iterator it; // it 能读写vector<int>的元素

string::iterator it2; // it2 能读写string对象中的字符

vector<int>::const_iterator it3; // it3 只能读元素，不能写元素

string::const_iterator it4; // it4 只能读字符，不能写字符
```

#### begin 和 end 运算符
`cbegin()`函数返回的是const_iterator

#### 结合解引用和成员访问操作
**箭头运算符（->）**将解引用和成员访问两个操作结合在一起
```c++
for (auto it = text.cbegin(); it != text.cend() && !it -> empty(); ++it)
{
  cout << *it << endl;
}
```

### 迭代器运算
`iter1 - iter2`:两个迭代器相减的结果是它们之间的距离，其类型名为**difference_type**，是一个带符号整型数。

```c++
// 二分查找
auto begin = text.begin(), end = text.end();
auto mid = text.begin() + (end - begin) / 2;

while (mid != end && *mid != sought)
{
  if (sought < *mid)
    end = mid;
  else
    begin = mid + 1;
  
  mid = begin + (end - begin) / 2;
}
```

