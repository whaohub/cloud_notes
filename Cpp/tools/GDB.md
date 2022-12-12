[toc]

# GDB

## -g 加入调试信息

-g选项并不是将源代码嵌入到目标文件中，gdb调试的时候也需要源文件。
所以应该在源文件目录下的cmake文件中添加相应语句，使得gdb调试时，源代码与可执行文件中的机器代码能够对应的上。
如果一个函数调用已经用内联替换优化过了，那么任何对这个调用进行跟踪或设置断点的尝试都会失败(我没有去验证过...记录于<<深入理解计算机操作系统>>)

```
Not able to set gdb breakpoint : 无法设置断点
You neglected to specify -g on the link line (some platforms require -g both at compile and link time),
You have a "stray" -s somewhere on your link line (which strips the final executable).
```

## 断点

保存断点 

```
save breakpoints gdb.cfg
Saved to file 'gdb.cfg'.
```

使用“source”命令restore（恢复，还原）它们（断点）

```
(gdb) source gdb.cfg 
```

## gdb调试记住相关配置参数

在当前用户下新建.gdbinit文件
将需要设置的参数在.gdbinit中设置好

```
# 每次调试时, 如果程序需要指定参数, 也可以在.gdbinit中提前
# 配置好, 这样每次gdb时就不用重复输入了
# 这里根据自己要调试程序的参数自定义设置, 
# 不需要的可以删除或使用"#"注释掉
 set args -D db1 start


directory ../ki/    //指定源码搜索路径
```

* set environment

```c
gdb your_program

(gdb) set environment LD_PRELOAD ./yourso.so
```

## gdb optimized out 问题

gdb调试程序的时候打印变量值会出现<value optimized out> 情况,可以在gcc编译的时候加上 -O0参数项,意思是不进行编译优化,调试的时候就会顺畅了,运行流程不会跳来跳去的,发布项目的时候记得不要在使用-O0参数项,gcc 默认编译或加上-O2优化编译会提高程序运行速度.
这意味着您使用了编译优化进行编译，gcc -O3并且gcc优化器发现某些变量在某种程度上是多余的，从而使它们得以优化。例如gcc -O0，如果您想查看此类变量，则在禁用优化的情况下进行编译（在任何情况下，这通常都是调试构建的一个好主意）
较新的gcc具有一个选项，-Og它仅应用那些不会损害可调试性的优化-非常有用（也man gcc适用于-gdwarf4）。另外，如果您不想对其进行编译器优化，但又不想为整个构建禁用优化，则可以将不想丢失的变量临时定义volatile

* optimized out  https://www.xmodulo.com/print-optimized-out-value-gdb.html
* What's the difference between a compiler's `-O0` option and `-Og` option https://stackoverflow.com/questions/63386189/whats-the-difference-between-a-compilers-o0-option-and-og-option/63386263#63386263

##  gdb 不显示线程启动和退出

可以使用`set print thread-events off`命令，这样当有线程产生和退出时，就不会打印提示信息

Tags:
  gdb