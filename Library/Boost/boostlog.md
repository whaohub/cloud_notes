# Boost Log

## boost_log tutorial

```c++
#include <boost/log/trivial.hpp>

int main(int, char*[])
{
    BOOST_LOG_TRIVIAL(trace) << "A trace severity message";
    BOOST_LOG_TRIVIAL(debug) << "A debug severity message";
    BOOST_LOG_TRIVIAL(info) << "An informational severity message";
    BOOST_LOG_TRIVIAL(warning) << "A warning severity message";
    BOOST_LOG_TRIVIAL(error) << "An error severity message";
    BOOST_LOG_TRIVIAL(fatal) << "A fatal severity message";

    return 0;
}
```

编译命令:

```
g++ -std=c++11 -DBOOST_LOG_DYN_LINK -c boosttest.cpp
g++ boosttest.o -lboost_log  -o boosttest
```

boost_log优点:

1. 除记录消息外，输出中的每个日志记录都包含时间戳，当前线程标识符和严重性级别。

2. 从不同线程同时编写日志是安全的，日志消息不会损坏。

3. 如稍后所示，可以应用过滤

加入日志文件保存及过滤:

```c++
#include <boost/log/core.hpp>
#include <boost/log/trivial.hpp>
#include <boost/log/expressions.hpp>
#include <boost/log/utility/setup/file.hpp>

namespace logging = boost::log;

void init()
{
    logging::add_file_log("sample.log");   //logging to a file


    logging::core::get()->set_filter       //添加日志过滤
    (
        logging::trivial::severity >= logging::trivial::warning
    );
}
```

编译参数修改:

```makefile
boost_log:boost_log.o
    g++ boost_log.o -w -lboost_filesystem -lboost_log -lboost_system -lboost_program_options -lboost_thread -lpthread -o boost_log

boost_log.o:boost_log.cpp
    g++ -std=c++11 -DBOOST_LOG_DYN_LINK -c boost_log.cpp
```
