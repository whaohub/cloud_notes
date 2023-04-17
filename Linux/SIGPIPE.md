# SIGPIPE

## handle SIGPIPE

在服务端运行程序并于mq通信时遇到[Segfault at "write () ../sysdeps/unix/syscall-template.S:84"](https://stackoverflow.com/questions/56679149/segfault-at-write-sysdeps-unix-syscall-template-s84)  经查询是未处理SIGPIPE信号;默认情况下这个信号会终止整个进程

## 在什么场景下会产生`SIGPIPE`信号

如果一个 socket 在接收到了 RST packet 之后，程序仍然向这个 socket 写入数据，那么就会产生`SIGPIPE`信号

’对一个服务端已经收到FIN包的socket调用read方法, 如果接收缓冲已空, 则返回0, 这就是常说的表示客户端连接关闭. 但第一次对其调用write方法时, 如果发送缓冲没问题, 会返回正确写入(发送). 但发送的报文会导致对端(客户端)发送RST报文, 因为对端的socket已经调用了close, 完全关闭, 既不发送, 也不接收数据. 所以, 第二次调用write方法(假设在收到RST之后), 会生成SIGPIPE信号, 导致进程退出

## 要怎样处理`SIGPIPE`信号？

对 server 来说，为了不被`SIGPIPE`信号杀死，那就需要忽略`SIGPIPE`信号：

```cpp
int main()
{
   signal(SIGPIPE, SIG_IGN); //SIG_IGN specifies that the signal should be ignored. 
}
```



## 信号处理



```c++

#include <signal.h>
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>

void my_handler(int s)
{
    printf("Caught signal %d\n",s);
    quit_flags = 1; 
    thread_status = false;

}
int main()
{
    struct sigaction sigIntHandler;

    sigIntHandler.sa_handler = my_handler;
    sigemptyset(&sigIntHandler.sa_mask);
    sigIntHandler.sa_flags = 0;

    sigaction(SIGINT, &sigIntHandler, NULL);

 }

```



[ref0](https://senlinzhan.github.io/2017/03/02/sigpipe/)