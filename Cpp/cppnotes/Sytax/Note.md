# C++ 运算符号

## C 运算符

## | 和 ||，& 和 && 的区别‘

我们将 || 和 && 定义为逻辑运算符，而 | 和 & 定义为位运算符。

&& 如果两个操作数都非零，则条件为真；

|| 如果两个操作数中有任意一个非零，则条件为真。

& 按位与操作，按二进制位进行"与"运算。运算规则：（有 0 则为 0）

```
0&0=0;   
0&1=0;    
1&0=0;     
1&1=1;
```

| 按位或运算符，按二进制位进行"或"运算。运算规则：（有 1 则为 1）

```
0|0=0;   
0|1=1;   
1|0=1;    
1|1=1;
```

## 此声明没有存储类或类型说明符”错误分析

定义全局变量往往需要把变量定义在函数体外部，若此时对变量进行赋值就会出现

“此声明没有存储类或类型说明符”的错误

## 获取cpu架构

```c
extern "C" {
    const char *getBuild() { //Get current architecture, detectx nearly every architecture. Coded by Freak
        #if defined(__x86_64__) || defined(_M_X64)
        return "x86_64";
        #elif defined(i386) || defined(__i386__) || defined(__i386) || defined(_M_IX86)
        return "x86_32";
        #elif defined(__ARM_ARCH_2__)
        return "ARM2";
        #elif defined(__ARM_ARCH_3__) || defined(__ARM_ARCH_3M__)
        return "ARM3";
        #elif defined(__ARM_ARCH_4T__) || defined(__TARGET_ARM_4T)
        return "ARM4T";
        #elif defined(__ARM_ARCH_5_) || defined(__ARM_ARCH_5E_)
        return "ARM5"
        #elif defined(__ARM_ARCH_6T2_) || defined(__ARM_ARCH_6T2_)
        return "ARM6T2";
        #elif defined(__ARM_ARCH_6__) || defined(__ARM_ARCH_6J__) || defined(__ARM_ARCH_6K__) || defined(__ARM_ARCH_6Z__) || defined(__ARM_ARCH_6ZK__)
        return "ARM6";
        #elif defined(__ARM_ARCH_7__) || defined(__ARM_ARCH_7A__) || defined(__ARM_ARCH_7R__) || defined(__ARM_ARCH_7M__) || defined(__ARM_ARCH_7S__)
        return "ARM7";
        #elif defined(__ARM_ARCH_7A__) || defined(__ARM_ARCH_7R__) || defined(__ARM_ARCH_7M__) || defined(__ARM_ARCH_7S__)
        return "ARM7A";
        #elif defined(__ARM_ARCH_7R__) || defined(__ARM_ARCH_7M__) || defined(__ARM_ARCH_7S__)
        return "ARM7R";
        #elif defined(__ARM_ARCH_7M__)
        return "ARM7M";
        #elif defined(__ARM_ARCH_7S__)
        return "ARM7S";
        #elif defined(__aarch64__) || defined(_M_ARM64)
        return "ARM64";
        #elif defined(mips) || defined(__mips__) || defined(__mips)
        return "MIPS";
        #elif defined(__sh__)
        return "SUPERH";
        #elif defined(__powerpc) || defined(__powerpc__) || defined(__powerpc64__) || defined(__POWERPC__) || defined(__ppc__) || defined(__PPC__) || defined(_ARCH_PPC)
        return "POWERPC";
        #elif defined(__PPC64__) || defined(__ppc64__) || defined(_ARCH_PPC64)
        return "POWERPC64";
        #elif defined(__sparc__) || defined(__sparc)
        return "SPARC";
        #elif defined(__m68k__)
        return "M68K";
        #else
        return "UNKNOWN";
        #endif
    }
}
```

## 打印颜色输出

```c
#define ZC_NONE         "\033[m"
#define ZC_RED          "\033[0;32;31m"
#define ZC_LIGHT_RED    "\033[1;31m"
#define ZC_GREEN        "\033[0;32;32m"
#define ZC_LIGHT_GREEN  "\033[1;32m"
#define ZC_BLUE         "\033[0;32;34m"
#define ZC_LIGHT_BLUE   "\033[1;34m"
#define ZC_DARY_GRAY    "\033[1;30m"
#define ZC_CYAN         "\033[0;36m"
#define ZC_LIGHT_CYAN   "\033[1;36m"
#define ZC_PURPLE       "\033[0;35m"
#define ZC_LIGHT_PURPLE "\033[1;35m"
#define ZC_BROWN        "\033[0;33m"
#define ZC_YELLOW       "\033[1;33m"
#define ZC_LIGHT_GRAY   "\033[0;37m"
#define ZC_WHITE        "\033[1;37m"

fprintf(stdout, "%s func %s() line %d," ZC_RED "cannot found engine:" ZC_NONE "%s\n", __FILE__, __FUNCTION__, __LINE__, enginePath.c_str());
```

## 打印函数名

[GCC](https://so.csdn.net/so/search?q=GCC&spm=1001.2101.3001.7020) 预定义了两个标识符来保存当前函数的名称。 标识符 `__FUNCTION__` 包含函数在源代码中出现的名称。 [标识符](https://so.csdn.net/so/search?q=标识符&spm=1001.2101.3001.7020) `__PRETTY_FUNCTION__` 包含以特定语言方式漂亮打印的函数名称。



Tags:
  daily, Note