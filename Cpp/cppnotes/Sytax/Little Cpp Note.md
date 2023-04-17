# Little Cpp Note

## calcuate time

统计程序运行时间 

```cpp
#define START_TIMER auto start = std::chrono::high_resolution_clock::now(); 返回
#define STOP_TIMER(print_message) std::cout << print_message << \
    std::chrono::duration_cast<std::chrono::milliseconds>( \
    std::chrono::high_resolution_clock::now() - start).count() \
    << " ms " << std::endl;
```

## Get_timestamp fun

获取时间戳函数 精确到毫秒:

```cpp
    static get_timestamp()
    {
        guint64 tmp;
        struct timeval tv;

        gettimeofday(&tv, NULL);
        tmp = tv.tv_sec;
        tmp = tmp * 1000;
        tmp = tmp + (tv.tv_usec / 1000);
        g_print("Timestamp:%lld\n", tmp);
        return tmp;
    }
```

## find_if 查找pair

```cpp
auto iter = std::find_if(m_dqImage.cbegin(), m_dqImage.cend(),
                                 [&temptime](const std::pair<veu64, IplImage *> &element)
                                 { return temptime - element.first >= 0 && temptime - element.first <= 3; });
```



## ck 函数

```cpp
inline bool check(int e, int iLine, const char *szFile) {
    if (e < 0) {
        LOG(ERROR) << "General error " << e << " at line " << iLine << " in file " << szFile;
        return false;
    }
    return true;
}

#define ck(call) check(call, __LINE__, __FILE__)
```



## 遇到的问题记录

char int 保存数值问题

在对象内生命了static 变量去记录上一次状态作对比,发现多对象时static变量多对象共享,造成异想不到的问题

又犯了了一个错误使用deque迭代器时候,将迭代器的end()当成了最后一元素,应该是back()

Tags:
  function, math, time, tool