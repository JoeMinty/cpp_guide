 ```c++
 class allocator {
 
  allocate() { ::operator new(); };
  
  deallocate() { ::operator delete(); };
 }
 ```
 
 **想要使用std::allocator以外的allocator，得自行#include <ext/...>**
 
 ```c++
 
 ```
 
