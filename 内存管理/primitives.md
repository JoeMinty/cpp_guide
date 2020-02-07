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
