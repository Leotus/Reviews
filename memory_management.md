```
注:
ctor - 构造函数
dtor - 析构函数
```

# primitives

## 内存分配层面

```
C++ Application
|   |  |- C++ Library(std::allocator)
|   |       |
|   |- C++ primitives(new,new[],new(),::operator new(),...)
|   |   |
|   |- CRT(malloc / free)
|   |
|- O.S.API(such as HeapAlloc,VirtualAlloc,...)

```

## 四个层面的基本用法测试

- 以下前三种好理解,最后一种直接绕开了 stl 的容器使用分配器,而且存在编译器差异

```
    void* p1 = malloc(512);	//512 bytes
    free(p1);

    complex<int>* p2 = new complex<int>; //one object
    delete p2;

    void* p3 = ::operator new(512); //512 bytes
    ::operator delete(p3);

//以下使用 C++ 標準庫提供的 allocators。
//其接口雖有標準規格，但實現廠商並未完全遵守；下面三者形式略異。
#ifdef _MSC_VER
    //以下兩函數都是 non-static，定要通過 object 調用。以下分配 3 個 ints.
    int* p4 = allocator<int>().allocate(3, (int*)0);// 这个(int*)0是没用的,但是要写
    allocator<int>().deallocate(p4,3);
#endif
#ifdef __BORLANDC__
    //以下兩函數都是 non-static，定要通過 object 調用。以下分配 5 個 ints.
    int* p4 = allocator<int>().allocate(5);
    allocator<int>().deallocate(p4,5);
#endif
#ifdef __GNUC__
    //以下兩函數都是 static，可通過全名調用之。以下分配 512 bytes.
    //void* p4 = alloc::allocate(512);
    //alloc::deallocate(p4,512);

    //以下兩函數都是 non-static，定要通過 object 調用。以下分配 7 個 ints.
	void* p4 = allocator<int>().allocate(7);
    allocator<int>().deallocate((int*)p4,7);

    //以下兩函數都是 non-static，定要通過 object 調用。以下分配 9 個 ints.
	void* p5 = __gnu_cxx::__pool_alloc<int>().allocate(9);
    __gnu_cxx::__pool_alloc<int>().deallocate((int*)p5,9);
#endif

```

## 基础构件之一 new delete expression

```
Complex* pc = new Complex(1,2);
```

编译器转化为

```
Complex *pc;
try{
  void* mem = operator new(sizeof(Complex));
  pc = static_cast<Complex*>(mem);
  pc->Complex::Complex(1,2);
  // 注意:只有编译器才可以像上面那样直接调用构造函数
  // 欲直接调用构造函数,可以使用placement new:
  // new(p)Complex(1,2);
}
catch(std::bad_alloc){
  // 若allocation失败就不执行constructor
}
```

```
delete pc;
```

编译器转化为

```
pc->~Complex(); // 先析构,可以直接调用析构函数
operator delete(pc); // 再释放内存
```

## array new array delete

```
Complex* pca = new Complex[3];
// 唤起三次ctor
// 无法通过参数给予初值

delete[] pca; // 唤起3次dtor
```

如果我 delete pca,没对每个 object 调用 dtor,有啥影响?

- 对 class without ptr member 可能没影响.(也就是说上个例子中 Complex 类中没有指针,没加[]delete 也不会发生内存泄漏)
- 对 class with pointer member 通常有影响.(下面的例子中 string 类中有指针,没加[]会发生内存泄漏)

```
string* psa = new string[3];
...
delete psa; // 唤起1次dtor
```

所以,内存泄漏发生在类内的指针指向的内存,不是一开始那个指针指向的内存

```
class A
{
public:
  int id;

  A() : id(0)      { cout << "default ctor. this="  << this << " id=" << id << endl;  }
  A(int i) : id(i) { cout << "ctor. this="  << this << " id=" << id << endl;  }
  ~A()             { cout << "dtor. this="  << this << " id=" << id << endl;  }
};
```

```
//回頭測試單純的 array new
  A* buf = new A[size];  //default ctor 3 次. [0]先於[1]先於[2]
          //A必須有 default ctor, 否則 [Error] no matching function for call to 'jj02::A::A()'
  A* tmp = buf;

cout << "buf=" << buf << "  tmp=" << tmp << endl;

  for(int i = 0; i < size; ++i)
    new (tmp++) A(i);  		//3次 ctor,placement new稍后解释

cout << "buf=" << buf << "  tmp=" << tmp << endl;

delete [] buf;    //dtor three times (次序逆反, [2]先於[1]先於[0])

```

## placement new

- placement new 允许我们将 object 建构在已经分配好的内存中
- 没有所谓的 placement delete,因为 placement new 根本没分配 memory

```
#include <new>
char* buf = new char[sizeof(Complex)*3];
Coomplex* pc = new(buf)Complex(1,2);
...
delete [] buf;
```

编译器转化为

```
Complex *pc;
try{
  void* mem = operator new(sizeof(Complex),buf);// 注意这里的buf,和之前不同,这里的重载和之前不同,这里不分配实际内存
  pc = static_cast<Complex*>(mem);
  pc->Complex::Complex(1,2);
  // 注意:只有编译器才可以像上面那样直接调用构造函数
  // 欲直接调用构造函数,可以使用placement new:
  // new(p)Complex(1,2);
}
catch(std::bad_alloc){
  // 若allocation失败就不执行constructor
}
```

构造完,pc 和 buf 指向同一个内存地址

## 重载 operator new、operator delete

### C++应用程序,分配内存途径梳理

- 最初应用程序

```
Foo* p = new Foo(x);
...
delete p;
```

- 编译器表达式转化为

```
Foo* p = (Foo*)operator new(sizeof(Foo));
new(p) Foo(x);
...
p->Foo;
operator delete(p);
```

1. 若无重载,执行 global funcions(同样可以重载,但是非常少见)

```
::operator new(size_t);

::operator delete(void*)
```

- 然后调用 CRT functions

```
malloc(size_t)

free(void*)
```

2. 存在重载,调用 member functions

```
Foo::operator new(size_t)

Foo::operator delete(void*)
```

- 然后再调用 global functions

```
::operator new(size_t);

::operator delete(void*)
```

- 然后调用 CRT functions

```
malloc(size_t)

free(void*)
```

### 重载::operator new/::operator delete(全局)

!!!小心,这样的影响非常深远,建议不用

```
void* myAlloc(size_t size)
{ return malloc(size); }

void* myFree(void* ptr)
{ return free(ptr); }

// 他们不能被声明在一个namespce内,这里没有做什么具体的事情,和原本的global functions做的没太大区别,只为了演示
inline void* operator new(size_t size)
{return myAlloce( size );}

inline void* operator new[](size_t size)
{return myAlloce( size );}

inline void* operator delete(void* ptr)
{ myFree( ptr );}

inline void* operator delete[](void* ptr)
{ myFree( ptr );}
```

### 重载 operator new/operator delete(类内)(较有用)

形式示例:

```
class Foo{
public:
  void* operator new(size_t);
  void operator delete(void*,size_t);// size_t是optional的
  //...
}
```

array 版本:

```
class Foo{
public:
  void* operator new[](size_t);
  void operator delete[](void*,size_t);// size_t是optional的
  //...
}
```

# malloc/free

# std::allocator

# other allocators

# loki::allocator
