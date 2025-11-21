static_cast\<target-type﻿>(expression ﻿)
# Пример
```cpp
#include <iostream>
#include <vector>
 
struct B
{
    int m = 42;
    const char* hello() const
    {
        return "Hello world, this is B!\n";
    }
};
 
struct D : B
{
    const char* hello() const
    {
        return "Hello world, this is D!\n";
    }
};
 
enum class E { ONE = 1, TWO, THREE };
enum EU { ONE = 1, TWO, THREE };
 
int main()
{
    // 1. static downcast
    D d;
    B& br = d; // upcast via implicit conversion
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "1) " << br.hello();
    D& another_d = static_cast<D&>(br); // downcast
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "1) " << another_d.hello();
 
    // 3. lvalue to xvalue
    [std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int> v0{1, 2, 3};
    [std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int> v2 = static_cast<[std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int>&&>(v0);
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "3) after move, v0.size() = " << v0.size() << '\n';
 
    // 4. discarded-value expression
    static_cast<void>(v2.size());
 
    // 5. initializing conversion
    int n = static_cast<int>(3.14);
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "5) n = " << n << '\n';
    [std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int> v = static_cast<[std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int>>(10);
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "5) v.size() = " << v.size() << '\n';
 
    // 6. inverse of implicit conversion
    void* nv = &n;
    int* ni = static_cast<int*>(nv);
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "6) *ni = " << *ni << '\n';
 
    // 7a. scoped enum to int
    E e = E::TWO;
    int two = static_cast<int>(e);
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "7a) " << two << '\n';
 
    // 7b. int to enum, enum to another enum
    E e2 = static_cast<E>(two);
    [[maybe_unused]]
    EU eu = static_cast<EU>(e2);
 
    // 7f. pointer to member upcast
    int D::*pm = &D::m;
    [std::cout](https://en.cppreference.com/w/cpp/io/cout.html) << "7f) " << br.*static_cast<int B::*>(pm) << '\n';
 
    // 7g. void* to any object pointer
    void* voidp = &e;
    [[maybe_unused]]
    [std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int>* p = static_cast<[std::vector](https://en.cppreference.com/w/cpp/container/vector.html)<int>*>(voidp);
}
```
