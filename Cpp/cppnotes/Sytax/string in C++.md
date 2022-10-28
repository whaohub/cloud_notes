# char* vs std:string vs char[] in C++

本文将在C++中三个不同的初始化string的方式并比较他们之间的差异。

## Using char *

[geeksforgeeks]: https://www.geeksforgeeks.org/char-vs-stdstring-vs-char-c/    "char* vs std:string vs char[] in C++"

## char* && char[] to string

 **`char[]` to string using `std::string()`**

```cpp
char name[] = {"Ironman"};

std::string strName = std::string(name);
```

 **`char*` to `string`**

```cpp
char *name = "Captain America";

std::string strName = std::string(name);
```

or

```cpp
const char *name = "Thor";

std::string str(name);
```
