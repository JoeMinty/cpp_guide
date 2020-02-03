# 函数
## 函数基础
一个典型的函数定义包括以下部分

- 返回类型

- 函数名字

- 由0个或多个形参组成的列表

- 函数体（函数执行的操作的语句块）

**编写函数**

**函数调用**

函数的调用完成两项工作

- 用实参初始化函数对应的形参

- 将控制权转移给调用函数（此时主调函数的执行被暂时中断，被调函数开始执行）

当遇到return语句时函数结束执行过程。return语句也完成两项工作

- 返回return语句中的值

- 将控制权从被调函数转移回主调函数（函数的返回值用于初始化调用表达式的结果，之后继续完成调用所在的表达式的剩余部分）

**形参和实参**

实参是形参的初始值；实参的类型必须与对应的形参类型匹配

**函数的形参列表**

```c++
void f1() {}; // 隐式地定义空形参列表

void f2(void) {}; // 显式地定义空形参列表
```

形参名可选；每个形参都含有一个声明符的声明

**函数返回类型**

- void，表示函数不返回任何值

- 返回值不能是数组类型或函数类型

- 指向数组或函数的指针

### 局部对象
形参和函数体内部定义的变量统称为**局部变量（local variable）**，局部变量会隐藏在外层作用域中同名的其他所有声明中

#### 自动对象
只存在于块执行期间的对象称为自动对象。

形参是一种自动对象。函数开始时为形参申请存储空间，函数一旦终止，形参也就被销毁

#### 局部静态对象
局部静态对象在程序执行路径第一次经过对象定义语句时初始化，并且直到程序终止才被销毁，在此期间即使对象所在的函数结束执行也不会对它有影响

### 函数声明
函数声明也称为函数原型

#### 在头文件中进行函数声明
含有函数声明的头文件应该包含到定义函数的源文件中

### 分离式编译
分离式编译允许把程序分割到几个文件中去，每个文件独立编译。

#### 编译和链接多个源文件
编译器负责把对象文件链接在一起形成可执行文件

## 参数传递
当形参是引用类型时，对应的实参被 **引用传递（passed by reference）** 或者函数被 **传引用调用（called by reference）**，引用形参是它绑定的对象的别名，引用形参是它对应的实参的别名

当实参的值被拷贝给形参时，形参和实参是两个相互独立的对象。这样的实参被 **值传递（passed by value）** 或者函数被 **传值调用（called by value）**

### 传值参数
当初始化一个非引用类型的变量时，初始值被拷贝给变量。

#### 指针形参
拷贝的是指针的值，拷贝之后，两个指针是不同的指针。

### 传引用参数
引用形参绑定初始化它的对象

#### 使用引用避免拷贝
如果函数无需改变引用形参的值，最好将其声明成常量引用

#### 使用引用形参返回额外信息

### const形参和实参
#### 指针或引用形参与const

#### 尽量使用常量引用

### 数组形参
数组有两个特殊的性质

- 不允许拷贝数组

- 使用数组时通常会将其转换成指针

### 返回数组指针
**声明一个返回数组指针的函数**

**`Type (* function (parameter_list)) [dimension]`**

```c++
int (*func(int i))[10];

// func(int i) 表示调用func函数时需要一个int类型的实参

// *func(int i) 表示可以对函数调用的结果执行解引用操作
 
// (*func(int i))[10] 表示解引用func函数的调用将得到一个大小是10的数组

// int (*func(int i))[10] 表示数组中的元素是int类型
```

**使用尾置返回类型**
```c++
auto func(int n) -> int(*)[10];
```

**使用decltype**
```c++
string s[10];
decltype(s) *func(int i);
```

## 函数重载
如果同一作用域内的几个函数名字相同但形参列表不同，称之为**重载函数**

**定义重载函数**
不允许两个函数除了返回类型外其他所有的要素都相同
```c++
int lookup(const Account&);

bool lookup(const Account&); // 错误
```

**判断两个形参的类型是否相异**

**重载和const形参**

**const_cast和重载**

**调用重载的函数**

### 重载与作用域

## 特殊用途语言特性
### 默认实参
**默认实参声明**

**默认实参初始值**

局部变量不能作为默认实参

### 内联函数和constexpr函数
**内联函数可避免函数调用的开销**

内联机制用于优化规模较小、流程直接、频繁调用的函数

**constexpr函数**

是指能用于常量表达式的函数

**把内联函数和constexpr函数放在头文件内**

## 函数匹配
**确定候选函数和可行函数**

**寻找最佳匹配**

**含有多个形参的函数匹配**

## 函数指针
**使用函数指针**

当把函数名作为一个值使用时，该函数自动地转换成指针。

**重载函数的指针**

**函数指针形参**

**返回指向函数的指针**

**将auto和decltype用于函数指针类型**



































































