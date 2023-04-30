## cmake 基础变量

### project

设置项目名称

Sets the name of the project, and stores it in the variable [`PROJECT_NAME`](https://cmake.org/cmake/help/latest/variable/PROJECT_NAME.html#variable:PROJECT_NAME). When called from the top-level `CMakeLists.txt` also stores the project name in the variable [`CMAKE_PROJECT_NAME`](https://cmake.org/cmake/help/latest/variable/CMAKE_PROJECT_NAME.html#variable:CMAKE_PROJECT_NAME).



### CXX compile feature marco

> CMAKE_CXX_STANDARD
>
> 设置cxx 标准

```cmake
sample:
set(CMAKE_CXX_STANDARD 11)
```



> CXX_STANDARD_REQUIRED
>
> 描述cxx 标准是否是一个必须的设置条件

```cmake
sample: 
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```

>CMAKE_CXX_EXTENSIONS
>
>This property specifies whether compiler specific extensions should be used. For some compilers, this results in adding a flag such as `-std=gnu++11` instead of `-std=c++11` to the compile line. This property is `ON` by default

```cmake
sample:
set(CMAKE_CXX_EXTENSIONS OFF)
```



## cmake cross compile

samples: crosscmake.cmake

```cmake
# the name of the target operating system
set(CMAKE_SYSTEM_NAME Windows)

# which compilers to use for C and C++
set(CMAKE_C_COMPILER   i586-mingw32msvc-gcc)
set(CMAKE_CXX_COMPILER i586-mingw32msvc-g++)

# where is the target environment located
set(CMAKE_FIND_ROOT_PATH  /usr/i586-mingw32msvc
    /home/alex/mingw-install)

# adjust the default behavior of the FIND_XXX() commands:
# search programs in the host environment
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

# search headers and libraries in the target environment
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```



```cmake
cmake -DCMAKE_TOOLCHAIN_FILE=~/Toolchains/crosscmake.cmake ..
```

