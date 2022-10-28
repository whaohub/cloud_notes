# More Effective C++ （20211119）

## 条款 十九（了解临时对象来源）

程序员交谈时，往往把一个短暂需要的变量称为“临时变量”
eg:

```
template<typename T>
void swap(T& object1, T& object2)
{
    T temp = object1;
    object1 = object2;
    object2 = temp; 
}
```
    常有人将temp称为一个临时对象。但在c++眼中，temp并不是临时对象，他只是一个函数中的局部对象。

C++真正的所谓的临时对象是不可见的--只要你产生一个non-heap object 而没有为他命名，便诞生了一个临时对象。通常发生与俩种情况：
 1. 当隐式类型转换被施行起来以求函数调用能够成功;
 2. 当函数返回对象的时候。
 了解这些临时对象如何被产生和被销毁，是很重要的，因为其所伴随的构造成本和析构成本可能对你的程序性能带来值得注意的影响

 首先考虑“为了让函数调用成功”而产生的临时对象。此乃发生与“传递某对象给一个函数”，而其类型与他绑定上去的参数类型不同的时候。

 ```
 eg 1:
 size_t countChar(const std::string& str, char ch)

char buffer[10];
char c;
std::cin >> c >> std::setw(10) >> buffer;
std::cout << "there are" << countChar(buffer, c) << "occurrences of the character"

eg2:
 char subtileBook[] = "Effective C++";
uppercasify(buffer);  //error

void uppercasify(std::string& str)
 ```

 ## 条款20: 协助完成 “返回值优化”

 ```
 inline const Rational operator* (const Rational & lhs, const Rational& rhs )
 {
    return Rational(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
 }
 ```

 ## 条款21:利用重载技术避免隐式类型转换

 C++中每个重载操作符必须获得至少一个用户定制类型的自变量。

 ## 条款22: 考虑以操作符复合形式（op=）取代其独身形式

 ```
 x = x + y;
 x+=y;
 ```

 ## 条款23: 考虑使用其他程序库
eg:
 iostream compare   stdio 
 

Tags:
  C++, MoreEffective