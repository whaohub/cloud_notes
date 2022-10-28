# Valgrind 使用

## 工具简介

Valgrind包括如下一些工具：

* Memcheck：这是valgrind应用最广泛的工具，一个重量级的内存检查器，能够发现开发中绝大多数内存错误使用情况，比如：使用未初始化的内存，使用已经释放了的内存，内存访问越界等。

* Callgrind：它主要用来检查程序中函数调用过程中出现的问题。

* Cachegrind：它主要用来检查程序中缓存使用出现的问题。

* Helgrind：它主要用来检查多线程程序中出现的竞争问题。

* Massif：它主要用来检查程序中堆栈使用中出现的问题。

* Extension：可以利用core提供的功能，自己编写特定的内存调试工具。

## --tool=memcheck

### Overview

Memcheck 是一个内存检查工具,可以检查以下出现在c/c++ 中的问题

- Accessing memory you shouldn't, e.g. overrunning and underrunning heap blocks, overrunning the top of the stack, and accessing memory after it has been freed.
- Using undefined values, i.e. values that have not been initialised, or that have been derived from other undefined values.
- Incorrect freeing of heap memory, such as double-freeing heap blocks, or mismatched use of `malloc`/`new`/`new[]` versus `free`/`delete`/`delete[]`
- Overlapping `src` and `dst` pointers in `memcpy` and related functions.
- Passing a fishy (presumably negative) value to the `size` parameter of a memory allocation function.
- Memory leaks.

### Explanation of error messages from Memcheck

#### 1. Illegal read / Illegal write errors

非法读/写错误: 

eg:

```shell
Invalid read of size 4
   at 0x40F6BBCC: (within /usr/lib/libpng.so.2.1.0.9)
   by 0x40F6B804: (within /usr/lib/libpng.so.2.1.0.9)
   by 0x40B07FF4: read_png_image(QImageIO *) (kernel/qpngio.cpp:326)
   by 0x40AC751B: QImageIO::read() (kernel/qimage.cpp:3621)
 Address 0xBFFFF0E0 is not stack'd, malloc'd or free'd
```

[Memcheck]:https://valgrind.org/docs/manual/mc-manual.html

## Leak Summary 各项解析

eg:

```
= LEAK SUMMARY:
==441648==    definitely lost: 369 bytes in 8 blocks
==441648==    indirectly lost: 5,631 bytes in 213 blocks
==441648==      possibly lost: 0 bytes in 0 blocks
==441648==    still reachable: 21,647 bytes in 118 blocks
==441648==         suppressed: 0 bytes in 0 blocks
==441648== Reachable blocks (those to which a pointer was found) are not shown.
==441648== To see them, rerun with: --leak-check=full --show-leak-kinds=all
==441648== 
==441648== For lists of detected and suppressed errors, rerun with: -s
==441648== ERROR SUMMARY: 8 errors from 8 contexts (suppressed: 0 from 0)


==454692== 
==454692== HEAP SUMMARY:
==454692==     in use at exit: 21,647 bytes in 118 blocks
==454692==   total heap usage: 1,053 allocs, 935 frees, 2,762,483 bytes allocated
==454692== 
==454692== LEAK SUMMARY:
==454692==    definitely lost: 0 bytes in 0 blocks
==454692==    indirectly lost: 0 bytes in 0 blocks
==454692==      possibly lost: 0 bytes in 0 blocks
==454692==    still reachable: 21,647 bytes in 118 blocks
==454692==         suppressed: 0 bytes in 0 blocks
==454692== Reachable blocks (those to which a pointer was found) are not shown.
==454692== To see them, rerun with: --leak-check=full --show-leak-kinds=all
==454692== 
==454692== For lists of detected and suppressed errors, rerun with: -s
==454692== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

| 类型              | 解析          |
| --------------- | ----------- |
| definitely lost | 确定泄露        |
| indirectly      | 间接泄露        |
| possibly        | 可能泄露        |
| still reachable | 未free, 但仍可用 |

### Running your program under Memcheck

基本：

```
valgrind --leak-check=yes myprog arg1 arg2
```

## Valgrind 性能分析功能

### 使用工具callgrind进行性能分析

* 使用方法
  在linux下，使用命令valgrind --tool=callgrind execname
  如果是多线程，可以增加选项--separate-threads=yes
  会在当前目录下生成文件，文件名字为“callgrind.out.进程号"
  如果为多线程，则会生成多个文件。
* 文件解析
  一种为转换为dot文件，然后再将dot文件转换为图片。
  第一、生成dot文件
  gprof2dot.py -f callgrind -n10 -s callgrind.out.31113 > valgrind.dot
  第二、将dot文件转换成图片
  dot -Tpng valgrind.dot -o valgrind.png

linux 下另一种方式为使用软件 kcachegrind

## cppcheck 代码检查

静态代码检查，使用方法

```
cppcheck --enable=all [files or paths]，重点看error打印
```

默认情况下只显示错误信息，可以通过“--enable”命令来启动更多检查，可用命令如下：

```
--enable=error         #发现bug时提示级别
--enable=style         #编码格式问题，未使用的函数、多余的代码等
--enable=portability   #打开移植性警告，在其它平台上可能出现兼容性问题
--enable=warning       #打开警告消息
--enable=performance   #打开性能消息
--enable=information   #打开信息消息
--enable=all           #打开所有消息
```



## valgrind arm版本安装

本文记录的应用场景是使用 Valgrind 调试嵌入式平台（目标系统）如 aarch64 下的应用，宿主系统的环境是 Linux X64。首先获取 Valgrind 的源码，在宿主机上交叉编译：

| 1<br><br>2<br><br>3<br><br>4<br><br>5 | $ apt-get source valgrind<br><br>$ cd valgrind<br><br>$ ./configure --host=aarch64-linux-gnu --prefix=$HOME/output/valgrind-aarch64 \<br><br>  CC=aarch64-linux-gnu-gcc CXX=aarch64-linux-gnu-c++<br><br>$ make && make install |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

注：这里的 –host 参数指定的是目标系统的架构， –prefix 参数指定软件的安装路径。

然后将编译好的 valgrind 拷贝到目标机上，如果拷贝到目标机上的路径和上面编译时指定的 –prefix 不一致，运行时需要设置 valgrind 运行时的 LIB 路径：

| 1   | $ export VALGRIND_LIB=/tmp/nfs/valgrind-aarch64/libexec/valgrind |
| --- | ---------------------------------------------------------------- |

Tags:
  Valgrind