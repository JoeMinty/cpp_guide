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






