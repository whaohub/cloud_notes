# PIMPL 编译防火墙

## Reasons for using the PIMPL idiom:

### Binary compatibility

当您开发库时，您可以将字段添加到XIMPL，而不会打破客户端的二进制兼容性（这意味着崩溃！）。由于X类的二进制布局在XIMPL类中添加新字段时不会更改，因此可以安全地将新功能添加到次要版本更新中的库中。

当然，您还可以将新的 pubilc/private non-virtual methods 添加到X/XIMPL中，而无需破坏二进制兼容性。

### Data hiding

如果您正在开发一个库，尤其是专有库，则可能不希望暴露其他库 /实施技术到公共接口中。要么是因为知识产权问题，要么是因为您认为用户可能会想对实施做出危险的假设，或者只是通过使用可怕的铸造技巧来打破封装。PIMPL解决/缓解。

### Compilation time

减少编译时间，因为在将X的字段和/或方法添加到XIMPL类中时，只有X的源文件（实现）文件需要重建（在XIMPL类中添加映射到标准技术中添加私有字段/方法）。实际上，这是一个普遍的操作。

使用 header/implementation技术（无PIMPL），当您向X添加新字段时，每个曾经分配X的客户端（在堆栈上或在堆上）需要重新编译，因为它必须调整分配的大小。好吧，每个从未分配X的客户端也都需要重新编译，但它只是不必要的开销（客户端的结果代码将是相同的）。

此外，即使将私有方法x :: foo（）添加到x和x.h，即使XCLIENT1.CPP无法调用此方法，也需要重新编译标准标头/实现分隔XCLIENT1.CPP。封装原因！像上面一样，它是纯粹的开销

### pImpl 技法的替代方案是

- 内联实现：私有成员和公开成员是同一类的成员

- 纯虚类（OOP 工厂）：用户获得到某个轻量级或纯虚的基类的唯一指针，实现细节则处于覆盖其虚成员函数的派生类中。

#### 运行时开销

- 访问开销：pImpl 中，每次对私有成员函数的调用都通过一个指针间接进行。私有成员对公开成员的每次访问也都通过另一个指针间接进行。两个访问都跨翻译单元边界，从而只能被连接时优化优化掉。注意 OO 工厂对公开数据和实现细节的访问都要求跨翻译单元间接进行，而且由于虚派发而给予连接时优化器更少的机会。

- 空间开销：pImpl 添加一个指针到公开组分，并且若有任何私有成员需要访问公开成员，则要么添加另一个指针到实现组分，要么每次调用要求它的私有成员时作为参数传递。若支持有状态自定义分配器，则必须一同存储分配器实例。

- 生存期管理开销：pImpl（还有 OO 工厂）将实现对象放在堆上，这在构造与销毁时强加了显著的运行时开销。这可以部分地由自定义分配器所弥补，因为 pImpl（但非 OO 工厂）的大小在编译时是已知的。
  
  另一方面，pImpl 类是对移动友好的；把大型的类重构为可移动 pImpl，可以提升对保有这些对象的容器进行操作的算法性能，即便可移动 pImpl 也具有额外的运行时开销：任何在被移动对象上容许使用并需要访问私有实现的公开成员函数必须进行空指针检查。

eg: test.h

```c
#ifndef __TEST__H__
#define __TEST__H__
#include <memory>
#include <string>
class Test
{
public:
    Test(std::string str);
    ~Test();
    Test(const Test&);
    Test& operator=(Test other);
    void Display();
private:
    class Impl;
    std::unique_ptr<Impl> pimpl;
};

#endif
```

test.cpp

```c
#include "Test.h"
#include <stdio.h>
struct Test::Impl
{
    Impl(std::string str)
        :name(std::move(str))
        {
            printf("Impl ctor\n");
        }

    ~Impl() = default;

    void PrintName()
    {
        printf("name = %s\n",name.c_str());
    }
    std::string name;
};

Test::Test(std::string str)
    :pimpl(new Impl(str))
    {
        printf("Test ctor\n");
    }
Test::~Test() = default;

Test::Test(const Test& other)
    :pimpl(new Impl(*other.pimpl)){}

Test& Test::operator=(Test other)
{
    std::swap(pimpl, other.pimpl);
    return *this;
}

void Test::Display()
{
    pimpl->PrintName();
}
```

[stackoverflow]:[https://stackoverflow.com/questions/8972588/is-the-pimpl-idiom-really-used-in-practice]()
[cpp_references]:[https://www.apiref.com/cpp-zh/cpp/language/pimpl.html]()
[geeksforgeeks]:[https://www.geeksforgeeks.org/pimpl-idiom-in-c-with-examples/]()
