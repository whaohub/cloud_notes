# Linkers And Loaders

## 一 编译和链接

    在构建一个程序时可以分解为以下四个步骤，预处理(Prepressing),编译(Compilation),汇编(Assembly),链接(Linking).

### 1.1 预编译

## 目标文件中都有什么

.text 代码段 程序源代码编译后的机器指令代码
.data 数据段 保存初始化的全局静态变量和局部静态变量。
.rodata 只读数据段 存放的程序里的只读数据，一般是程序里面的const修饰的变量，字符串常量
.bss       存放的是未初始化的全局变量和局部静态变量
自定义段 GCC 提供一个扩展机制，使得程序员指定变量所处的段
我们在全局变量或函数之前加上如下属性，就可以把相应的变量或函数放到“name” 作为段名的段中

```
__attribute__((section("FOO"))) int global = 10
__attribute__((section("BAR"))) void foo()
```

## 链接器如何解析多重定义的全局符号

函数和已初始化的全局变量是强符号，未初始化的全局变量是弱符号
根据强弱符号的定义，Linux链接器使用如下规则处理多重定义的符号名

* rule1 :不允许有多个同名的强符号。
* rule2 :如果有一个强符号和多个弱符号重名，那么选择强符号。
* rule3:如果有多个弱符号同名，那么从这些弱符号中任意选择一个。

## 查看目标文件结构命令

* objdump文章：https://ivanzz1001.github.io/records/post/linux/2018/04/09/linux-objdump

```
$ objdump -h SimpleSection.o

SimpleSection.o：     文件格式 elf64-x86-64

节：
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         00000061  0000000000000000  0000000000000000  00000040  2**0
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, CODE
  1 .data         00000008  0000000000000000  0000000000000000  000000a4  2**2
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00000008  0000000000000000  0000000000000000  000000ac  2**2
                  ALLOC
  3 .rodata       00000008  0000000000000000  0000000000000000  000000ac  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  4 .comment      0000002b  0000000000000000  0000000000000000  000000b4  2**0
                  CONTENTS, READONLY
  5 .note.GNU-stack 00000000  0000000000000000  0000000000000000  000000df  2**0
                  CONTENTS, READONLY
  6 .note.gnu.property 00000020  0000000000000000  0000000000000000  000000e0  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  7 .eh_frame     00000058  0000000000000000  0000000000000000  00000100  2**3
                  CONTENTS, ALLOC, LOAD, RELOC, READONLY, DATA
```

* size obj.o    查看目标文件各段的长度

```
用法：size [选项] [文件]
 显示二进制文件中节的大小
 没有给出输入文件，默认为 a.out
 The options are:
  -A|-B|-G  --format={sysv|berkeley|gnu}  Select output style (default is berkeley)
  -o|-d|-x  --radix={8|10|16}         Display numbers in octal, decimal or hex
  -t        --totals                  Display the total sizes (Berkeley only)
            --common                  Display total size for *COM* syms
            --target=<bfdname>        Set the binary file format
            @<file>                   Read options from <file>
  -h        --help                    Display this information
  -v        --version                 Display the program's version
```

```
$ size SimpleSection.o
   text       data        bss        dec        hex    filename
    225          4         16        245         f5    SimpleSection.o
```

* readlf 命令详细查看ELF文件
  
  ```
  $ readelf -h  SimpleSection.o
  ELF 头：
  Magic：   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  类别:                              ELF64
  数据:                              2 补码，小端序 (little endian)
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI 版本:                          0
  类型:                              REL (可重定位文件)
  系统架构:                          Advanced Micro Devices X86-64
  版本:                              0x1
  入口点地址：               0x0
  程序头起点：          0 (bytes into file)
  Start of section headers:          5936 (bytes into file)
  标志：             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           0 (bytes)
  Number of program headers:         0
  Size of section headers:           64 (bytes)
  Number of section headers:         23
  Section header string table index: 22
  ```

```
* nm 命令查看符号表
 需要加入-g编译
```

$ nm SimpleSection.o 
0000000000000000 D global_init_foo_var
0000000000000000 B global_init_var
                 U _GLOBAL_OFFSET_TABLE_
0000000000000004 B global_uninit_var
0000000000000028 T main
                 U printf
0000000000000000 T _Z4funci
0000000000000008 b _ZZ4mainE15static_init_var
000000000000000c b _ZZ4mainE17static_uninit_var

```
* strip 命令
strip命令可以去掉ElF文件中的调试信息，以节省大量空间
```

```
$ strip SimpleSection.o 

# wh @ ThinkPad in /UbuntuDisk/Whaofiles/cppPractice/c++ prictice/LinkersAndLoaders/staticLink/src [10:35:13]

$ objdump -h SimpleSection.o

SimpleSection.o：     文件格式 elf64-x86-64

节：
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         0000007a  0000000000000000  0000000000000000  00000040  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, CODE
  1 .data         00000000  0000000000000000  0000000000000000  000000ba  2**0
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00000010  0000000000000000  0000000000000000  000000bc  2**2
                  ALLOC
  3 FOO           00000004  0000000000000000  0000000000000000  000000bc  2**2
                  CONTENTS, ALLOC, LOAD, DATA
  4 .rodata       00000023  0000000000000000  0000000000000000  000000c0  2**0
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  5 .comment      0000002b  0000000000000000  0000000000000000  000000e3  2**0
                  CONTENTS, READONLY
  6 .note.GNU-stack 00000000  0000000000000000  0000000000000000  0000010e  2**0
                  CONTENTS, READONLY
  7 .note.gnu.property 00000020  0000000000000000  0000000000000000  00000110  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
  8 .eh_frame     00000058  0000000000000000  0000000000000000  00000130  2**3
                  CONTENTS, ALLOC, LOAD, READONLY, DATA
```

### 测试程序目录 dir : /c++ prictice/LinkersAndLoaders/staticLink/src

a.c

```
extern int shared;

int main()
{
    int a = 100;
    swap(&a, &shared);
    return 0;
}
```

b.c

```
int shared = 1;

void swap(int *a, int *b)
{
    int temp;
    temp = *a;
    *a = *b;
    *b = temp; 
}
```

编译：

```
$ gcc -c a.c b.c 
```

编译中发现定义a文件如果为cpp文件，编译报错  error: ‘swap’ was not declared in this scope
文章有时间看一下:cnblogs.com/zjutzz/p/10802138.html

## GCC创建和使用静态库

Linux下的静态库是以.a结尾的二进制文件，他作为程序的一个模块，在链接期间被组合到程序中。
动态链接库（.so文件），他在程序运行阶段加载进内存

Linux下静态库命令规则

```
libxxx.a
```

* gcc/g++ 生成静态链接库
  静态库可以简单的看成一组目标文件的集合，即很多目标文件经过压缩打包后形成的一个文件。
1. 首先使用gcc命令将源文件编译为目标文件，也即.o文件
   
   ```
   
   ```

gcc -c *.c

```
2. 然后使用ar命令将.o文件打包成静态库文件
```

ar rcs + 静态库文件的名字 + 目标文件列表

```
ar 是 Linux 的一个备份压缩命令，它可以将多个文件打包成一个备份文件（也叫归档文件），也可以从备份文件中提取成员文件。ar 命令最常见的用法是将目标文件打包为静态链接库。

对参数的说明：
参数 r 用来替换库中已有的目标文件，或者加入新的目标文件。
参数 c 表示创建一个库。不管库否存在，都将创建。　
参数 s 用来创建目标文件索引，这在创建较大的库时能提高速度。

使用ar命令查看这个文件包含了哪些目标文件
```

ar -t libc.a  

```
* 使用动态库
使用静态链接库时，除了需要库文件本身，还需要对应的头文件：库文件包含了真正的函数代码，也即函数定义部分；头文件包含了函数的调用方法，也即函数声明部分

在编译 main.c 的时候，我们需要使用-I（大写的字母i）选项指明头文件的包含路径，使用-L选项指明静态库的包含路径，使用-l（小写字母L）选项指明静态库的名字。所以，main.c 的完整编译命令为：
```

gcc src/main.c -I include/ -L lib/ -l test -o math.out

```
注意，使用-l选项指明静态库的名字时，既不需要lib前缀，也不需要.a后缀，只能写 test，GCC 会自动加上前缀和后缀。

## ldd 命令

常用来查看共享库的依赖关系
格式:
ldd [ 选项  ] 文件

选项:
--version: 打印ldd的版本号
-v --verbose: 打印所有信息，例如包括符号的版本信息
-d --data-relocs: 执行符号重部署， 并报告缺少的目标对象(只对ELE格式的文件适用)
-r --function-relocs:对目标对象和函数执行重新部署，并报告缺少的目标对象和函数(只对ELE格式的文件适用)
[Linking](simplenote://note/098efcf5-0662-4e14-8e28-165aeacdf402)

查看链接器搜索路径

 ld --verbose | grep SEARCH

Tags:
  C++, Linking