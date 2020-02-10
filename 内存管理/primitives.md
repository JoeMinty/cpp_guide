## new expression
```c++
Complex* pc = new Complex(1, 2);
```

编译器转换为
```c++
Complex *pc;
try {
  void* mem = operator new( sizeof(Complex) );
  pc = static_cast<Complex*> (mem);
  pc->Complex::Complex(1, 2);
}
catch (std::bad_alloc) {

}


void *operator new(size_t size, const std::nothrow_t&) 
{
  void *p;
  while ((p = malloc(size)) == 0) 
  {
    _TRY_BEGIN
      if (_callnewh(size) == 0) break;
    _CATCH(std::bad_alloc) return (0);
    _CATCH_END
  }
  return p;
}
```

## delete expression
```c++
Complex* pc = new Complex(1, 2);
...
delete pc;
```

编译转换为

```c++
pc->~Complex(); // 先析构
operator delete(pc); // 然后释放内存

void __cdecl operator delete(void *p) _THROW0() 
{
  free(p);
}

```

## array new, array delete

## placement new
placement new 允许我们将对象构造在已经存在的内存中

`placement delete` 对应 `operator delete`

```c++
#include <new>
char* buf = new char[sizeof(Complex) * 3];
Complex* pc = new(buf)Complex(1, 2);
...
delete[] buf;
```

编译转换为

```c++
 Complex *pc;
 try {
  void* mem = operator new (sizeof(Complex), buf);
  pc = static_cast<Complex*>(mem);
  pc->Complex::Complex(1, 2);
 } 
 catch( std::bad_alloc ) {
 }
 
 void* operator new(size_t, void* loc) {
  return loc;
 }
```


## 重载
```c++
class Foo {
public:
  void* operator new(size_t);
  void operator delete(void*, sizet_t);
}
```

**demo1**
```c++
#include <iostream>
#include <cstddef>
using namespace std;

class Screen {
public:
    Screen(int x): i(x) {};
    int get() {return i;}

    void* operator new(size_t);
    void operator delete(void*, size_t);

private:
    Screen* next;  // 多耗用一个next指针
    static Screen* freeStore;
    static const int screenChunk;
private:
    int i;
};

Screen* screen::freeStore = 0;
const int Screen::screenChunk = 24;

void* Screen::operator new(size_t size) 
{
    Screen* p;
    if (!freeStore) {
        size_t chunk = screenChunk * size;

        // 将一大块分割，当作linked list串起来
        freeStore = p = reinterpret_cast<Screen*>(new char[chunk]);

        for (; p != &freeStore[screenChunk - 1]; ++p) {
            p->next = p + 1;
        }

        p->next = 0;
    }

    p = freeStore;
    freeStore = freeStore->next;
    return p;
}

void Screen::operator delete(void *p, size_t)
{
    // 将deleted object 插回free list前端
    (static_cast<Screen*>(p))->next = freeStore;
    freeStore = static_cast<Screen*>(p); 
}
```

**demo2**
```c++
#include <iostream>
#include <cstddef>
using namespace std;

class Screen {
public:
    Screen(int x): i(x) {};
    int get() {return i;}

    void* operator new(size_t);
    void operator delete(void*, size_t);

private:
    Screen* next;  // 多耗用一个next指针
    static Screen* freeStore;
    static const int screenChunk;
private:
    int i;
};

Screen* screen::freeStore = 0;
const int Screen::screenChunk = 24;

void* Screen::operator new(size_t size) 
{
    Screen* p;
    if (!freeStore) {
        size_t chunk = screenChunk * size;

        // 将一大块分割，当作linked list串起来
        freeStore = p = reinterpret_cast<Screen*>(new char[chunk]);

        for (; p != &freeStore[screenChunk - 1]; ++p) {
            p->next = p + 1;
        }

        p->next = 0;
    }

    p = freeStore;
    freeStore = freeStore->next;
    return p;
}

void Screen::operator delete(void *p, size_t)
{
    // 将deleted object 插回free list前端
    (static_cast<Screen*>(p))->next = freeStore;
    freeStore = static_cast<Screen*>(p); 
}
```

**demo3  static allocator**
```c++
class allocator 
{
private:
    struct obj {
        struct obj* next;
    };
public:
    void* allocate(size_t);
    void deallocate(void*, size_t);
private:
    obj* freeStore = nullptr;
    const int CHUNK = 5;
}

void* allocator::allocate(size_t size)
{
    obj* p;
    if (!freeStore) {
        // linked list 为空，于是申请一大块内存
        size_t chunk = CHUNK * size;
        freeStore = p = (obj*) malloc(chunk);

        // 将分配得到的一大块当作linked list使用
        // 小块小块串起来
        for (int i = 0; i < CHUNK - 1; i++)
        {
            /* 嵌入式指针设计 */
            p->next = (obj*)((char*)p + size);
            p = p->next;
        }
        p->next = nullptr;
    }

    p = freeStore;
    freeStore = freeStore->next;
    return p;
}

void allocator::deallocate(void* p, sizt_t size)
{
    ((obj*)p)->next = freeStore;
    freeStore = (obj*)p;
}

class Foo {
public:
    long L;
    string str;
    static allocator myAlloc;
public:
    Foo(long l): L(l) {}
    static void* operator new(size_t size) {
        return myAlloc.allocate(size);
    }

    static void operator delete(void* pdead, size_t size) {
        return myAlloc.deallocate(pdead, size);
    }
}

allocator Foo::myAlloc;
```

**demo4 用宏取代**

## new handler
```c++
typedef void(*new_handler)();
new_handler set_new_handler(new_handler p) throw();
```

## operator new 和 delete的 delete default 修饰符
**delete** 不允许有的方法

**default** 用默认版本
