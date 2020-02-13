Object Oriented Programming

Object Oriented Design

### 继承（Inheritance）is-a

构造由内而外，drived的构造函数首先调用base的default构造函数，然后才执行自己；析构由外而内，drived的析构函数首先执行自己，然后调用base的析构函数

base class 的析构函数必须是virtual，不然会出现undefined behavior

### 复合 （Composition） has-a
**复合关系下的构造和析构**

构造由内而外，Container的构造函数首先调用Component的default构造函数，然后才执行自己；析构由外而内，Container的析构函数首先执行自己，然后调用Component的析构函数



```c++
Container::Container(...): Component() { ... };

Container::~Component(...) { ... ~Container(...);}
```

Container <>------> Component

### 委托 （Delegation） Composition by reference

### 转换函数 conversion function

### 智能指针

### pointer like class

### function like class

### namespace

### 类模板

### 函数模版

### 成员模版






















