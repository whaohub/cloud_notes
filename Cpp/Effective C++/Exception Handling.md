# Exception Handling

   异常处理机制允许程序中独立开发的部分能够在运行时就出现的问题进行通信并作出相应的处理。异常使得我们能够将问题的检测与解决过程分离开来。程序的一部分负责检测问题的出现，然后解决该问题的任务传递给程序的另一部分
   当执行一个throw时，跟在throw后面的语句将不再执行。程序的控制权从throw转移到与之匹配的catch模块。该catch可能是同一函数中的局部catch，也可能位于直接或间接调用了发生异常的函数的另一个函数中，控制权从一处转移到另一处，这有俩个重要的意义

* 沿着调用链的函数可能会提早退出.
* 一旦程序开始执行异常处理的代码，则沿着调用链创建的对象将被销毁
  因为跟在throw后面的语句不再执行，所以throw语句的用法有点类似return语句，他通常作为条件语句的一部分或作为函数最后一条语句

eg: 错误

```cpp
    void handle_fun()
{ 
        std::cout<<"handle_fun"<<std::endl;
        throw;
 }

try
        {
            pt->handle_fun();
        }
        catch(const char* e)  //捕捉所有异常
        {
            std::cout<<"exception catch "<<e <<std::endl;
            delete pt;
            throw;   // 将异常传播给调用端
        }
 terminate called without an active exception
```

catch (...) block is fine, the problem is your program does not throw an exception.

There are two forms of throw expression:

throw <some-exception> create and throw an exception
throw re-throw the current exception

Tags:
  cpp, cprimer, exception