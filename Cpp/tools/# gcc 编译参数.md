# gcc 编译参数

## -fPIC

-fPIC 作用于编译阶段，告诉编译器产生与位置无关代码(Position-Independent Code)，
  则产生的代码中，没有绝对地址，全部使用相对地址，故而代码可以被加载器加载到内存的任意
  位置，都可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的

## -I (大写i)

include头文件非标准库中存在的也不是在当前文件夹下的，需要将地址用-I包含
例：-I /home/src/

## -L

命令格式为−L<库文件所在路径> −l<库名>

如果在路径 /usr/lib 下面有一个库文件叫做 libthrift.so, 我想在编译的时候链接它, 只需加上 −L/usr/lib  −lthrift

## -l(小写L)

对于库文件在 /lib、/usr/lib 和 /usr/local/lib 路径下的库, 使用更加简单的命令 −l 就不需要添加库文件路径即可

## -o

指定生成可执行文件的名称。使用方法为：g++ -o fileName file.cpp file.h
注意点：可执行文件不可与待编译或链接文件同名，否则会生成相应可执行文件且覆盖原编译或链接文件
如果不使用-o选项，则会生成默认可执行文件a.out

## -c

 只编译不链接，只生成目标文件

## -g

 添加gdb调试选项

## 警告参数

## -w

 -w：关闭编译时的警告

解释：编译后不显示任何warning，因为有时在编译之后编译器会显示一些例如数据转换之类的警告，而这些警告是可以忽略的

### -Wall

编译后显示所有警告

### -W

与-Wall类似，会显示警告，但是只显示 编译器认为 会 出现错误 的警告
在编译一些项目的时候可以-W和-Wall选项一起使用，感兴趣的可以自己尝试一下，两个命令的组合使用。

### -fno-common

在遇到多重定义的全局符号时，触发一个错误。

### -Werror

把所有警告都变为错误

### -Werror=

指定警告视为错误 Werror=overflow

```c++
error: overflow in conversion from 'int' to 'char' changes value from '300' to '','' [-Werror=overflow]
    5 |     char i = 300;
```

### -Wfatal-errors

在发生第一个错误时中止编译

Use every available and reasonable set of warning options. Some warning options only work with optimizations enabled, or work better the higher the chosen level of optimization is, for example [`-Wnull-dereference`](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wnull-dereference-367) with GCC.

You should use as many compilers as you can for your platform(s). Each compiler implements the standard slightly differently and supporting multiple will help ensure the most portable, most reliable code.

```shell
-Wall -Wextra -Wshadow -Wnon-virtual-dtor -pedantic - use these and consider the following (see descriptions below)

-pedantic - Warn on language extensions
-Wall -Wextra reasonable and standard
-Wshadow warn the user if a variable declaration shadows one from a parent context
-Wnon-virtual-dtor warn the user if a class with virtual functions has a non-virtual destructor. This helps catch hard to track down memory errors
-Wold-style-cast warn for c-style casts
-Wcast-align warn for potential performance problem casts
-Wunused warn on anything being unused
-Woverloaded-virtual warn if you overload (not override) a virtual function
-Wpedantic (all versions of GCC, Clang >= 3.2) warn if non-standard C++ is used
-Wconversion warn on type conversions that may lose data
-Wsign-conversion (Clang all versions, GCC >= 4.3) warn on sign conversions
-Wmisleading-indentation (only in GCC >= 6.0) warn if indentation implies blocks where blocks do not exist
-Wduplicated-cond (only in GCC >= 6.0) warn if if / else chain has duplicated conditions
-Wduplicated-branches (only in GCC >= 7.0) warn if if / else branches have duplicated code
-Wlogical-op (only in GCC) warn about logical operations being used where bitwise were probably wanted
-Wnull-dereference (only in GCC >= 6.0) warn if a null dereference is detected
-Wuseless-cast (only in GCC >= 4.8) warn if you perform a cast to the same type
-Wdouble-promotion (GCC >= 4.6, Clang >= 3.8) warn if float is implicit promoted to double
-Wformat=2 warn on security issues around functions that format output (ie printf)
-Wlifetime (only special branch of Clang currently) shows object lifetime issues
Consider using -Weverything and disabling the few warnings you need to on Clang

-Weffc++ warning mode can be too noisy, but if it works for your project, use it also.
```

## -D_GLIBCXX_USE_CXX11_ABI=0

If you get linker errors about undefined references to symbols that involve types in the `std::__cxx11` namespace or the tag `[abi:cxx11]` then it probably indicates that you are trying to link together object files that were compiled with different values for the _GLIBCXX_USE_CXX11_ABI macro. This commonly happens when linking to a third-party library that was compiled with an older version of GCC. If the third-party library cannot be rebuilt with the new ABI then you will need to recompile your code with the old ABI.

Not all uses of the new ABI will cause changes in symbol names, for example a class with a `std::string` member variable will have the same mangled name whether compiled with the old or new ABI. In order to detect such problems the new types and functions are annotated with the abi_tag attribute, allowing the compiler to warn about potential ABI incompatibilities in code using them. Those warnings can be enabled with the `-Wabi-tag` option

如果您获取有关涉及std :: __ cxx11命名空间或标签[abi：cxxx11]的符号的链接器错误，则可能表明您正在尝试将链接在一起的对象文件与__GLIBCXX_USE_CXX11_ABI宏。当链接到使用GCC较旧版本的第三方库时，通常会发生这种情况。如果无法使用新的ABI重建第三方库，则需要将代码与旧的ABI重新编译。

并非所有新ABI的用法都会引起符号名称的更改，例如，具有STD :: String Member变量的类，无论是使用旧或新ABI编译，都将具有相同的修补名称。为了检测此类问题，使用ABI_TAG属性注释了新的类型和功能，从而使编译器可以使用它们警告代码中潜在的ABI不兼容性。这些警告可以通过-wabi -tag opti启用

链接时的第三方库使用了旧版的abi版本编译,而在使用新版abi程序编译后造成程序crash

https://gcc.gnu.org/onlinedocs/libstdc++/manual/using_dual_abi.html

## 库搜索路径

### -wl , rpath

在makefile中一般默认的 lib 的加载路径是/lib /usr/lib  如果想要改变程序运行时的libs的加载路径 就需要用到 -wl , rpath 参数来添加lib 加载路径。

* rpath 和 runpath
  gcc version >= 7.5.0时,  链接器选项 -rpath 的行为发生改变，默认配置为 RUNPATH 而不是 RPATH；由于 RUNPATH 不适用于间接依赖的库,所以会报出无法找到共享库错误。
  解决方案除了可以修改环境变量外(不推荐)，可以使用 -Wl,--disable-new-dtags 选项来使链接器保持旧行为，即在 (gcc version >=7.5.0) 使用如下命令编译

```shell
 gcc -I. -o main main.c -Wl,--disable-new-dtags,--rpath,. -L. -lA -lB
```

也有 -Wl,--enable-new-dtags 选项来使链接器保持新行为
在搜索app的间接依赖库时，RPATH起作用，但RUNPATH不起作用。在使用RUNPATH的情况下，很可能还要再配合LD_LIBRARY_PATH一块使用。
其他问题：

app中默认使用RPATH还是RUNPATH是由什么决定的？
目前看是跟Linux的发行版本有关。例如，在Fedora34 + gcc10.2生成的也是RPATH。
如何控制生成RPATH还是RUNPATH？
链接时使用--enable-new-dtags可以固定生成RUNPATH，使用--disable-new-dtags可以固定生成RPATH。

[rpath vs runpath]:https://medium.com/obscure-system/rpath-vs-runpath-883029b17c45

[rpath vs runpath]:https://stackoverflow.com/questions/7967848/use-rpath-but-not-runpath

### -rpath-link 和 -L,-rpath 区别

-rpath-link 类似-L 功能，-rpath-link会在指定路径自动搜索间接依赖的库，而-L 不会

https://stackoverflow.com/questions/49138195/whats-the-difference-between-rpath-link-and-l

## O0, O1, O2,

O0 O1 表示在不影响编译速率的前提下尽可能的优化程序的大小和运行速率。

O2  表示在牺牲部分编译速率的前提下 支持配置优化参数的优化 尽可能的提高运行速率。

O3  表示 采取多项量算法 提高程序的运行速率（他不惜增大程序的大小）

Os 和O3一样只不过他不会为了以为的提高程序运行速率二曾大程序的大小。

## 函数级别链接

由于程序和库通常来讲都非常庞大，当我们需要用到某个目标文件中的任意一个函数或变量时，就必须要把它整个的链接进来，也就是那些没用用到的也被一起链接了起来。这样的后果是链接输出的文件很大。
编译器提供一种函数级链接的机制，让所有函数单独保存到一个段里。当链接需要用到某个函数时，他就将它合并到输出文件中，对于那些没有用到的函数则将他们抛弃。这个做法可以很大程度减少输出文件的长度，减少空间的浪费。但是这个优化选项会减慢编译和链接过程，因为连接器需要计算各个函数之间的依赖关系，并且各个函数都保存到独立的段中，目标函数的段的数量大大增加，重定位过程也会因为段的数量增加而变得复杂，目标文件随着段数量增加也会变得相对较大。
 GCC 编译器有俩个选项 “ffunction-sections” 和 “fdata-sections", 这俩个选项的作用就是将每个函数或变量保持到独立的段中。

## 清除符号信息

正常情况下编译出来的共享库或可执行文件里面带有符号信息和调试信息，但对于最终发布的版本来说，这些符号信息用处不大，并且会使文件尺寸变大，我们可以使用”strip“工具清楚共享库的符号信息和调试信息。
消除静态库时可能需要  --strip-unneeded参数，它确保strip掉的是没有用的符号，保留用于链接的符号，尽管--strip-unneeded不如-s清除的彻底，但是保留了很多有用的信息，确保该链接库是可用的。
 还可以使用ld的”-s“和"-S" 参数，区别是，"-S"消除调试信息，”-s“消除符号信息。
 我们也可以使用gcc中的"-Wl,s"和"-Wl,S"给ld传递这俩个参数

Tags:
  g++, gcc, 编译参数