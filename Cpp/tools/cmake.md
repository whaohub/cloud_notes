## CMake

- Cmake 报错问题
  error 1:
  
  ```cmake
  CMake Error: The current CMakeCache.txt directory /home/hd/Jitu/CloudStaring/cloudstaringSrc/linux_camera/VideoManager/CMakeCache.txt is different than the directory /home/zhuoshi/camera_pose_wkw/VideoStructure_alg/VideoManager where CMakeCache.txt was created. This may result in binaries being created in the wrong place. If you are not sure, reedit the CMakeCache.txt
  ```

```
进入build目录，删除CMakeCache.txt即可

error 2:
 If you are using cmake add following to your cmake file:
 ```cmake
 SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")
```

undefined reference to symbol 'pthread_create@@GLIBC_2.2.5'
//lib/x86_64-linux-gnu/libpthread.so.0: error adding symbols: DSO missing from command line

## Cmake 添加gdb 调试信息

```cmake
    SET(CMAKE_BUILD_TYPE "Debug")
    SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
    SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```

在CMakeLists.txt文件的开头部分增加上面的几个SET语法行，简单解释如下：

在cmake中有一个全局的环境变量，CMAKE_BUILD_TYPE，可以取Release或者Debug等值。然后可以通过设置CMAKE_CXX_FLAGS_DEBUG来设置在debug时的CXXFLAGS，这个值大家肯定都熟悉的哈。如果不需要添加调试信息，就直接修改CMAKE_BUILD_TYPE的值。

## cmake参数说明

- include_directories
  include_directories其实就是指定头文件目录，我们可以指定某个目录，表示所有的头文件都在下面。

- add_library
  就像注释中说的那样，add_library就是将指定的一些源文件打包成动态库和静态库，第一个参数就是生成库的名字，第二个参数是生成库的类型，后面的参数就都是源文件了，可以是之前定义的列表，也可以用1.cpp 2.cpp 3.cpp去指定。

- target_link_libraries
  target_link_libraries就是将之前打包的库，链接到生成的目标上，不然会出现光声明，没定义的错误，注意也可以直接指定库名，如target_link_libraries(main XXX.so)或target_link_libraries(main XXX.a)。

## CMake 查找第三方库实例(取自Nvidia-decode)

```cmake
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    find_package(PkgConfig REQUIRED)
    pkg_check_modules(PC_AVCODEC REQUIRED IMPORTED_TARGET libavcodec)
    pkg_check_modules(PC_AVFORMAT REQUIRED IMPORTED_TARGET libavformat)
    pkg_check_modules(PC_AVUTIL REQUIRED IMPORTED_TARGET libavutil)
    pkg_check_modules(PC_SWRESAMPLE REQUIRED IMPORTED_TARGET libswresample)

    set(NV_FFMPEG_HDRS ${PC_AVCODEC_INCLUDE_DIRS})
    find_library(AVCODEC_LIBRARY NAMES avcodec
            HINTS
            ${PC_AVCODEC_LIBDIR}
            ${PC_AVCODEC_LIBRARY_DIRS}
            )
    find_library(AVFORMAT_LIBRARY NAMES avformat
            HINTS
            ${PC_AVFORMAT_LIBDIR}
            ${PC_AVFORMAT_LIBRARY_DIRS}
            )
    find_library(AVUTIL_LIBRARY NAMES avutil
            HINTS
            ${PC_AVUTIL_LIBDIR}
            ${PC_AVUTIL_LIBRARY_DIRS}
            )
    find_library(SWRESAMPLE_LIBRARY NAMES swresample
            HINTS
            ${PC_SWRESAMPLE_LIBDIR}
            ${PC_SWRESAMPLE_LIBRARY_DIRS}
            )
    set(AVCODEC_LIB ${AVCODEC_LIBRARY})
    set(AVFORMAT_LIB ${AVFORMAT_LIBRARY})
    set(AVUTIL_LIB ${AVUTIL_LIBRARY})
    set(SWRESAMPLE_LIB ${SWRESAMPLE_LIBRARY})
endif()
```

## cmake 参数

```cmake
LINK_DIRECTORIES()
需要在add_executable()前定义
```
