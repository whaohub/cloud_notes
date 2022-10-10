## 双重指针作为函数指针

In short, in C when you pass something as a parameter, a *copy* will be passed to the function. Changing the copy doesn't affect the original value.

However, if the value is a pointer, what it *points to* can be changed. In this case, if you want to affect the pointer, you need to pass *a pointer to it* down to the function.

https://stackoverflow.com/questions/17007944/usage-of-double-pointers-as-arguments

## c 指针的NULL

在大多数的操作系统上，程序不允许访问地址为 0 的内存，因为该内存是操作系统保留的。然而，内存地址 0 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。但按照惯例，如果指针包含空值（零值），则假定它不指向任何东西。

## 指针和引用

https://www.codenong.com/cs109847691/