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
