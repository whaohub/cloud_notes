[toc]

# gdb 常用命令

### Stop GDB at the first line of main()

You started up your gdb instance using `gdb --args` and now you would like to set up a breakpoint. The problem is that, since no library is loaded except for your program, you will not be able to do that.

Solution is to use `start` command. Command `start` just puts a temporary breakpoint on the first line of `main` and runs the program. Program stops at the first line of `main` and there you can insert your breakpoints easier since all libraries have already been loaded.

```shell
Reading symbols from ./linked_list_test...done.
(gdb) start
Temporary breakpoint 1 at 0x40d770: file linked_list_test.cpp, line 76.
Starting program: /home/ivica/johnysswlab/2020-05-datacaching/linked_list_test
Temporary breakpoint 1, main (argc=1, argv=0x7fffffffe1a8) at linked_list_test.cpp:76
76      int main(int argc, char* argv[]) {
(gdb) 
```



### Command: until

Until is a useful little command. If your program is paused in the debugger, and you want to move it forward a few lines, instead of running `next` several times, type `until line_number`. It will move the debugger to that particular line in the source code.

```
Temporary breakpoint 1, main (argc=1, argv=0x7fffffffe1a8) at linked_list_test.cpp:76
76      int main(int argc, char* argv[]) {
(gdb) until 90
main (argc=<optimized out>, argv=<optimized out>) at linked_list_test.cpp:90
90          run_test<4, iterations, 4>(my_array);
(gdb)
```

### Command: ignore

When you are debugging a problem, it happens often that the problem reproduces after a breakpoint is hit exact number of times. For example, problem appears in the 1,072nd invocation of function `test_fun()`.

You can use `ignore` command to quickly reproduce this. Load your program into GDB and then:

```shell
(gdb) break linked_list.h:111
Breakpoint 2 at 0x407538: linked_list.h:111. (12 locations)
(gdb) ignore 2 200
Will ignore next 200 crossings of breakpoint 2.
```

Ignore command skips the breakpoint for the given number of times.

If you want to see how many times breakpoint is hit use `info breakpoints`, e.g:

```shell
(gdb) ignore 2 200
Will ignore next 200 crossings of breakpoint 2.
...
(gdb) info b
Num     Type           Disp Enb Address            What
2       breakpoint     keep y   <MULTIPLE>
        breakpoint already hit 201 times
```



### Commands: info locals && bt full

If you are looking for a value of local variable, you can print the value of all local variables in the current thread using info locals:

```
(gdb) info locals
current = 0x611100
block_empty = <optimized out>
next = <optimized out>
prev = 0x0
```

If you want to print local variables in backtrace command, use `bt full`:

```shell
(gdb) bt full
#0  linked_list<test_struct<4>, 4>::remove_if<run_test(std::vector<int>&) [with int linked_list_values = 4; int iterations = 1; int struct_size = 4]::<lambda(const test_struct<4>&)> > (this=0x7fffffffe050, condition=<optimized out>) at linked_list.h:116
        current = 0x611100
        block_empty = <optimized out>
        next = <optimized out>
        prev = 0x0
#1  run_test<4, 1, 4> (my_array=std::vector of length 50000, capacity 50000 = {...}) at linked_list_test.cpp:66
        i = <optimized out>
        my_list = {begin = 0x611100, end = 0x6f86d0}
        header = "Node size = 4|struct size = 4|"
        len = 50000
...
```

### Command: command

When defining a breakpoint you can use the `command` command to specify custom commands to execute when the breakpoint is hit. For example, if you want to print a content of a variable `len` and continue, this is the way to do it:

```shell
(gdb) break linked_list_test.cpp:52
Breakpoint 2 at 0x407455: linked_list_test.cpp:52. (12 locations)
(gdb) command 2
Type commands for breakpoint(s) 2, one per line.
End with a line saying just "end".
>print len
>continue
>end
```

### Dynamic printf

GDB offers an ability to add printfs to the program without a need to recompile it. This feature is called dynamic printfs. Internally it is a breakpoint paired with printf and continue commands. It looks like this:

```shell
dprintf location, formatting-string, expr1, expr2, ...
```

Location specifies a location where you want to put your dprintf (similar to location in break command). Formatting-string, expr1, expr2 etc. have the same meaning as in case of printf. Here is an example to put it into more perspective:

```shell
(gdb) dprintf 145, "First element is %d\n", array_regular[0]
Dprintf 2 at 0x55555555690a: file sorting.cpp, line 145.
...
First element is 0
```

### Command: thread apply all

If you are working in a multithreaded environment, you are for sure familiar with command `info threads` that list all the threads in the system. But you can do more. Command `thread apply all cmd` will apply `cmd` for each thread in the process, for example:

```
thread apply all bt // prints stack backtrace for each thread
thread apply all print $pc // prints value of $pc for each thread
```

If you are debugging some code with someone over e-mail, the first thing you should ask them is output of `thread apply all bt full`. This will print stack backtraces of all threads with all local variables, a very useful tool for investigation.

### Log gdb output to a file

You can log your debug session to a file and later review it when debugging is over or send it someone by e-mail. Just run `set logging on` to log everything to a file called gdb.txt. More information about logging are available [here](https://sourceware.org/gdb/current/onlinedocs/gdb/Logging-Output.html).

### 保存断点

```
save breakpoints gdb.cfg
Saved to file 'gdb.cfg'.
```

使用“source”命令restore（恢复，还原）它们（断点）

```
(gdb) source gdb.cfg 
```

[ref]:https://johnysswlab.com/gdb-a-quick-guide-to-make-your-debugging-easier/
