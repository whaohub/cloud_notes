[TOC]

# Copy/Move Constructors

程序中临时值和中间值经常会被创建, "move ctor"在c++11中的增加，可以使程序这些情况下提高速度:
eg: 测试类包含俩个构造函数

```cpp
class Obj
{
public:
    Obj()
    {
        std::cout<<"obj ctor"<<std::endl;
    }

    Obj(const Obj&){ std::cout<<"obj copy ctor" << std::endl; }


};

void vector_obj()
{
    std::vector<Obj> vc_obj;

    std::cout<<"push back first element"<<std::endl;
    vc_obj.push_back(Obj());

    std::cout<<"push back second element"<<std::endl;
    vc_obj.push_back(Obj());

    std::cout<<"push back third element"<<std::endl;
    vc_obj.push_back(Obj());

    std::cout<<"push back forth element"<<std::endl;
    vc_obj.push_back(Obj());

    std::cout<<"push back fifth element"<<std::endl;
    vc_obj.push_back(Obj());
}
```

output:

```cpp
push back first element
obj ctor
obj copy ctor
push back second element
obj ctor
obj copy ctor
obj copy ctor
push back third element
obj ctor
obj copy ctor
obj copy ctor
obj copy ctor
push back forth element
obj ctor
obj copy ctor
push back fifth element
obj ctor
obj copy ctor
obj copy ctor
obj copy ctor
obj copy ctor
obj copy ctor
```

程序显示了有多少次拷贝发生，当第一个元素创建时，发生一次构造，一次拷贝，但第二次插入元素 时发生了多次拷贝？我们加入更多输出输出信息就可以看出产生的原因。

```c++
void vector_obj()
{
    std::vector<Obj> vc_obj;

    std::cout<<"push back first element"<<std::endl;
    vc_obj.push_back(Obj());
    std::cout<<"vector capacity "<<vc_obj.capacity() << std::endl;

    std::cout<<"push back second element"<<std::endl;
    vc_obj.push_back(Obj());
    std::cout<<"vector capacity "<<vc_obj.capacity() << std::endl;

    std::cout<<"push back third element"<<std::endl;
    vc_obj.push_back(Obj());
    std::cout<<"vector capacity "<<vc_obj.capacity() << std::endl;

    std::cout<<"push back forth element"<<std::endl;
    vc_obj.push_back(Obj());
    std::cout<<"vector capacity "<<vc_obj.capacity() << std::endl;

    std::cout<<"push back fifth element"<<std::endl;
    vc_obj.push_back(Obj());
    std::cout<<"vector capacity "<<vc_obj.capacity() << std::endl;

}
```

output:

```
push back first element
obj ctor
obj copy ctor
vector capacity 1
push back second element
obj ctor
obj copy ctor
obj copy ctor
vector capacity 2
push back third element
obj ctor
obj copy ctor
obj copy ctor
obj copy ctor
vector capacity 4
push back forth element
obj ctor
obj copy ctor
vector capacity 4
push back fifth element
obj ctor
obj copy ctor
obj copy ctor
obj copy ctor
obj copy ctor
obj copy ctor
vector capacity 8
```

从输出可以看出，当第一个元素插入后，vector已经满了，所以插入第二个元素时，容器需要二倍扩容然后将元素拷贝到新容器中。

lets see what happens when we add move ctor.

```c++
class Obj
{
public:
    Obj()
    {
        std::cout<<"obj ctor"<<std::endl;
    }

    Obj(const Obj&){ std::cout<<"obj copy ctor" << std::endl; } //copy ctor

    Obj(const Obj&&){std::cout<<"obj move ctor" << std::endl;}  //move ctor
};
```

output:

```
push back first element
obj ctor
obj move ctor
vector capacity 1
push back second element
obj ctor
obj move ctor
obj copy ctor
vector capacity 2
push back third element
obj ctor
obj move ctor
obj copy ctor
obj copy ctor
vector capacity 4
push back forth element
obj ctor
obj move ctor
vector capacity 4
push back fifth element
obj ctor
obj move ctor
obj copy ctor
obj copy ctor
obj copy ctor
obj copy ctor
vector capacity 8
```

容器会优先使用move ctor,但是当新容器创建时仍然使用copy ctor拷贝数据到新容器。
如果我们把move ctor改为:

```
Obj(const Obj&&) noexcept {std::cout<<"obj move ctor" << std::endl;}  //move ctor
```

output:

```
push back first element
obj ctor
obj move ctor
vector capacity 1
push back second element
obj ctor
obj move ctor
obj move ctor
vector capacity 2
push back third element
obj ctor
obj move ctor
obj move ctor
obj move ctor
vector capacity 4
push back forth element
obj ctor
obj move ctor
vector capacity 4
push back fifth element
obj ctor
obj move ctor
obj move ctor
obj move ctor
obj move ctor
obj move ctor
vector capacity 8
```

所有的拷贝都会变为move ctor,  也就是在可能情况下vector会使用move提高效率， but the vector routines have tight specifications. In particular, there are circumstances where a move constructor's only used if it doesn't throw exceptions

## To Summarise

* vector 背后会产生大量拷贝，为了减少这些拷贝可以在一开始设定出vector的容量。
* 标准库算法提供了利用move Ctor  的机会。

Tags:
  copy/move, cpp11