# Linux 下静态库与动态库符号冲突问题

## 问题描述

依赖关系: 

```c++
Application -> lib3rd.so-> libcommon.a.1
  |
  libcommon.so.2
```

解释:

1. Application依赖一个lib3rd.so的动态库,而lib3rd的动态库使用静态链接了一个静态库libcommon.a.1(且版本或符号可能与libcommon..so.2有冲突)

2. Application同时也依赖了一个libcommon.so.2的动态库

## Library Code

- libCOMMON_STATIC.a version 1.0

```c++
Headers:
#ifndef __COMMON_H__
#define __COMMON_H__
int COMMON_OPEN();
#endif
```

```c++
Srcs:

#include "common_static.h"

#include <stdio.h>

int COMMON_OPEN()
{
    printf("[%s:%s:%d]\n", __FILE__, __FUNCTION__, __LINE__);
    return 0;
}
```

- 编译

```sh
#!/bin/bash
g++ -c -g -fPIC common_static.cpp -o common_static.o
ar crv libCOMMON_STATIC.a common_static.o
```

libCOMMON_SHARED.so version 2.0  假设版本2中的动态库存在与低版本不兼容的符号

```c++
// version 2.0
Headers:
#ifndef __COMMON_H__
#define __COMMON_H__
int COMMON_OPEN();
#endif
```

```c++
#include "common_shared.h"

#include <stdio.h>

int COMMON_OPEN()
{
    printf("conflict symbol\n");
    printf("[%s:%s:%d]\n", __FILE__, __FUNCTION__, __LINE__);
    return 0;
}
```

- Compile

```sh
#!/bin/bash
g++ -g -fPIC -shared common_shared.cpp -o libCOMMON_SHARED.so
```

### Test Application

Code:

```c
#include "lib3rd.h"
#include <unistd.h>
int main(int argc, char *argv[])
{
    CALL_COMMON_FUN;
    while(1) sleep(1000);
    return 0;
}
```

Compile:

```sh
#!/bin/bash
g++ -g  Application.cpp -o Application -I ./lib3rd  -Wl,--rpath=./lib3rd -L ./lib3rd -l3rd  
```
