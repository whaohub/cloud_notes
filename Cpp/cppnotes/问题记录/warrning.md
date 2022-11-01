# Warrning

## 编译警告问题记录

```shell
warning: ignoring #pragma comment  [-Wunknown-pragmas]
```

Regarding the *warnings* (not errors), the `#pragma` you are using is not supported by most compilers (Microsoft and Embarcadero compilers do). So just remove it altogether (you already link to the library in the project makefile), or at least disable it with an appropriate `#if/def`

```c++
#if defined(_MSC_VER) || defined(__BORLANDC__)
#pragma comment(lib,"Ws2_32.lib")
#endif
```

## warn note

- 格式化与参数类型不匹配

```shell
warning: format ‘%d’ expects argument of type ‘int’, but argument 9 has type ‘__suseconds_t {aka long 
int}’ [-Wformat=]
```

- 精度丢失

```shell
 warning: narrowing conversion of ‘((float)a.std::pair<_IplImage*, _VE_Rect_>::second._VE_Rect_::x * ratio_x)’ from ‘float’ to ‘int’ inside { } [-Wnarrowing]
 warning: narrowing conversion of ‘((float)a.std::pair<_IplImage*, _VE_Rect_>::second._VE_Rect_::x * ratio_x)’ from ‘float’ to ‘int’ inside { } [-Wnarrowing]
```

```c++
 warning: embedded ‘\0’ in format [-Wformat-contains-nul]
  std::sprintf(buf, "%s%s%03d\0", timeBuf, DevNo.c_str(), ID);
```

- 成员变量 的声明顺序和构造函数初始化顺序不同
  
  ```shell
   will be initialized after [-Wreorder]
  ```

- 无符号有与有符号错误比较
  
  ```shell
  : warning: comparison between signed and unsigned integer expressions [-Wsign-compare]
           if(pos = filepath.find_last_of("/") != -1)
           find_last_of() 查找失败返回npos
  ```

> | static const size_type npos = -1; |     |     |
> | --------------------------------- | --- | --- |
> |                                   |     |     |
> 
> 这是特殊值，等于 `size_type` 类型可表示的最大值。准确含义依赖于语境，但通常，期待 string 下标的函数以之为字符串尾指示器，返回 string 下标的函数以之为错误指示器。
> 
> ### 注意
> 
> 虽然定义使用 -1 ，由于[有符号到无符号隐式转换](https://www.apiref.com/cpp-zh/cpp/language/implicit_cast.html#.E6.95.B4.E6.95.B0.E8.BD.AC.E6.8D.A2)，且 [`size_type`](https://www.apiref.com/cpp-zh/cpp/string/basic_string.html) 是无符号整数类型， `npos` 的值是其所能保有的最大正值。这是指定任何无符号类型的最大值的可移植方式。

- 定义带有返回值的函数没有写返回值
  
  ```
   warning: control reaches end of non-void function [-Wreturn-type]
  ```

- if 语句中警告
  
  ```c++
   warning: suggest parentheses around assignment used as truth value [-Wparentheses]
           if(pos = (filepath.find_last_of("/") != std::string::npos))
  ```
  
  需修改为判断前使用扩哈 if((pos = (filepath.find_last_of("/")) != std::string::npo

## invalid new-expression of abstract class type

说明抽象类中有函数没有具体实现，无法实例化类
