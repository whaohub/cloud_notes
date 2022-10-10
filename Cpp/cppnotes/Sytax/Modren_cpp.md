# Morder_cpp 11/14/17/20

## 变量及其初始化

//c++ 17

//在if或switch条件中声明临时变量

```cpp
void print_temp_obj()

{

​    std::vector<int> vc = {1,2,2};

​    if(const std::vector<int>::iterator itr = std::find(vc.begin(), vc.end(), 1);

​    itr != vc.end())

​    {

​        puts("find success");

​    }

}
```

//c++ 11 

//对象初始化列表

```cpp
class Data

{

public:

​    std::vector<int> vc;

​    Data(std::initializer_list<int> list)

​    {

​        for(auto it = list.begin(); it != list.end(); ++it)

​        {

​            vc.push_back(*it);

​        }

​    }

​    void print()

​    {

​        for(auto it:vc)

​        {

​            printf("data = %d\n", it);

​        }

​    }

};
```

//C++17

//C++11 新增了 std::tuple 容器用于构造一个元组，进而囊括多个返回值。

//但缺陷是，C++11/14 并没有提供一种简单的方法直接从元组中拿到并定义元组中的元素，

//尽管我们可以使用 std::tie 对元组进行拆包，但我们依然必须非常清楚这个元组包含多少个对象，

//各个对象是什么类型，非常麻烦。使用auto类型推导

//C++17 完善了这一设定，给出的结构化绑定可以让我们写出这样的代码



```cpp
std::tuple<int, double, std::string> f()
{

​    return std::make_tuple(1,2.3, "str");

}

void print_tuple()
{

​    auto [x, y, z] = f();

​    std::cout<<"x = "<< x <<"y = "<< y << "z = "<< z <<std::endl;

}
```


