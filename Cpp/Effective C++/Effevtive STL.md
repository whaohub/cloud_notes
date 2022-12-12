[toc]

# Effective STL





## Item 4 用empty来代替检查size()是否为0

理由很简单：对于所有的标准容器，empty是一个常数时间的操作，但对于一
些list实现，size花费线性时间。



## Item 5 尽量使用区间成员函数代替它们的单元素兄弟



## Item 6 警惕C++最令人恼怒的解析

```cpp
一段时间了，你应该会遇到另一个这条规则的表象。有多少次你会看见这个错误？
class Widget {...}; // 假设Widget有默认构造函数
Widget w();// 嗯哦……
```

这并没有声明一个叫做w的Widget，它声明了一个叫作w的没有参数且返回Widget的函数。学会识别这个失言
（faux pas）是成为C++程序员的一个真正的通过仪式。



## Item 9 在删除选项中仔细选择

- 去除一个容器中有特定值的所有对象：
  如果容器是vector、string或deque，使用erase-remove惯用法。
  如果容器是list，使用list::remove。
  如果容器是标准关联容器，使用它的erase成员函数。
-  去除一个容器中满足一个特定判定式的所有对象：
  如果容器是vector、string或deque，使用erase-remove_if惯用法。
  如果容器是list，使用list::remove_if。
  如果容器是标准关联容器，使用remove_copy_if和swap，或写一个循环来遍历容器元素，当你把迭代
  器传给erase时记得后置递增它。
- 在循环内做某些事情（除了删除对象之外）：
  如果容器是标准序列容器，写一个循环来遍历容器元素，每当调用erase时记得都用它的返回值更新你
  的迭代器。
  如果容器是标准关联容器，写一个循环来遍历容器元素，当你把迭代器传给erase时记得后置递增它

## Item 13 尽量使用vector和string来代替动态分配的数组

你决定使用new来进行动态分配，你需要肩负下列职责：
1. 你必须确保有的人以后会delete这个分配。如果后面没有delete，你的new就会产生一个资源泄漏。

2. 你必须确保使用了delete的正确形式。对于分配一个单独的对象，必须使用“delete”。对于分配一个
    数组，必须使用“delete []”。如果使用了delete的错误形式，结果会未定义。在一些平台上，程序在
    运行期会当掉。另一方面，它会默默地走向错误，有时候会造成资源泄漏，一些内存也随之而去。

3. 你必须确保只delete一次。如果一个分配被删除了不止一次，结果也会未定义。

无论何时，你发现你自己准备动态分配一个数组（也就是，企图写“new T[...]”），你应该首先考虑使用一
   个vector或一个string。（一般来说，当T是一个字符类型的时候使用string，否则使用vector，但我们在本条款
   的后面将遇到的情况中，vector<char>可能是一个合理的设计选择。）vector和string消除了上面的负担，因为
   它们管理自己的内存。当元素添加到那些容器中时它们的内存会增长，而且当一个vector或string销毁时，它
   的析构函数会自动销毁容器中的元素，回收存放那些元素的内存。

## Item14 使用reserve来避免不必要的重新分配

1. 分配新的内存块，它有容器目前容量的几倍。在大部分实现中，vector和string的容量每次以2为因数增
    长。也就是说，当容器必须扩展时，它们的容量每次翻倍。
2. 把所有元素从容器的旧内存拷贝到它的新内存。
3. 销毁旧内存中的对象。
4. 回收旧内存。

- size()告诉你容器中有多少元素。它没有告诉你容器为它容纳的元素分配了多少内存。
- capacity()告诉你容器在它已经分配的内存中可以容纳多少元素。那是容器在那块内存中总共可以容纳
  多少元素，而不是还可以容纳多少元素。如果你想知道一个vector或string中有多少没有被占用的内
  存，你必须从capacity()中减去size()。如果size和capacity返回同样的值，容器中就没有剩余空间了，而
  下一次插入（通过insert或push_back等）会引发上面的重新分配步骤。
-  resize(Container::size_type n)强制把容器改为容纳n个元素。调用resize之后，size将会返回n。如果n小于
  当前大小，容器尾部的元素会被销毁。如果n大于当前大小，新默认构造的元素会添加到容器尾部。
  如果n大于当前容量，在元素加入之前会发生重新分配。
-  reserve(Container::size_type n)强制容器把它的容量改为至少n，提供的n不小于当前大小。这一般强迫
  进行一次重新分配，因为容量需要增加。（如果n小于当前容量，vector忽略它，这个调用什么都不
  做，string可能把它的容量减少为size()和n中大的数，但string的大小没有改变。在我的经验中，使用
  [1]
  reserve来从一个string中修整多余容量一般不如使用“交换技巧”

## Item 17 使用“交换技巧”来修整过剩容量

修整你的竞争者vector过剩容量的方法：

```cpp
vector<Contestant>(contestants).swap(contestants);

std::vector<int> vc = {0, 1, 2, 3, 4};
vc.push_back(6); ‣x: 6
std::cout<<vc.capacity()<<std::endl;   // output 10
std::vector<int>(vc).swap(vc); ‣x: vc
std::cout<<vc.capacity()<<std::endl;    //output 6

```

表达式vector<Contestant>(contestants)建立一个临时vector，它是contestants的一份拷贝：vector的拷贝构造函数
做了这个工作。但是，vector的拷贝构造函数只分配拷贝的元素需要的内存，所以这个临时vector没有多余的
容量。然后我们让临时vector和contestants交换数据，这时我们完成了，contestants只有临时变量的修整过的容
量，而这个临时变量则持有了曾经在contestants中的发胀的容量。在这里（这个语句结尾），临时vector被销
毁，因此释放了以前contestants使用的内存。

## Item 18 避免使用vector<bool>
