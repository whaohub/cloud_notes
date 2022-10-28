# HelloMakefile

#### 基本makefile 编写

```
OBJ = main.o    #定义变量
main:$(OBJ)     #目标文件：依赖文件  #命令
    g++ -o main $(OBJ) 
.PHONY:clean
clean:
    rm -f *.o
```

```makefile
## Makefile

CXX = g++

CXX_FLAGS += -std=c++14
CXX_FLAGS += -Wl,-rpath-link

INCLUDES += -I../xxxx/include                                           # 头路径
INCLUDES += -I../xxx/xxx/include

LINKS += -L../xxx/lib                                                   # 库路径
LINKS += -L../xxx/xxx/lib                                               

LIBS += -lcuda -lcurt -pthread                                          # 添加一些依赖库
LIBS += -lopencv_imgproc -lopencv_imgcodecs -lopencv_core -lopencv_dnn


# For debug build, use the command: `make debug=1' 
DEBUG ?= 1
ifeq ($(DEBUG), 1)
    CFLAGS =-DDEBUG
else
    CFLAGS=-DNDEBUG
endif


SRCS = test.cc
EXECUTABLE = test

$(EXECUTABLE): $(SRCS)                                                 # 编译指令
    $(CXX) $(SRCS) $(CXX_FLAGS) $(INCLUDES) $(LINKS) $(LIBS) -o $(EXECUTABLE)

.PHONY:clean
clean:                                                                 # make clean
    rm -f $(EXECUTABLE)
```

example:

```makefile
CC := @g++
CUCC := nvcc
ECHO := @echo
SRCDIR := src
OBJDIR := objs
BINDIR := ./workspace
LEAN := /datav/newbb/lean

# -gencode=arch=compute_60,code=sm_60
OUTNAME := ffmpeg_hw_decode
CFLAGS := -std=c++11 -m64 -fPIC -g -fopenmp -w -O3 -pthread
CUFLAGS := -std=c++11 -m64 -Xcompiler -fPIC -g -w -O3 -gencode=arch=compute_75,code=sm_75
INC_OPENCV := $(LEAN)/opencv4.2.0/include/opencv4 $(LEAN)/opencv4.2.0/include/opencv4/opencv $(LEAN)/opencv4.2.0/include/opencv4/opencv2
INC_FFMPEG := $(LEAN)/ffmpeg4.2/include
INC_CUDA := $(LEAN)/cuda10.2/include 
INCS := $(INC_OPENCV) $(INC_FFMPEG) $(INC_CUDA)
INCS := $(foreach inc, $(INCS), -I$(inc))

LIB_CUDA := $(LEAN)/cuda10.2/lib
LIB_FFMPEG := $(LEAN)/ffmpeg4.2/lib
LIB_OPENCV := $(LEAN)/opencv4.2.0/lib
LIBS := $(LIB_OPENCV) $(LIB_FFMPEG) $(LIB_CUDA)
RPATH := $(foreach lib, $(LIBS),-Wl,-rpath=$(lib))
LIBS := $(foreach lib, $(LIBS),-L$(lib))

LD_OPENCV := opencv_core opencv_highgui opencv_imgproc opencv_video opencv_videoio opencv_imgcodecs
LD_CUDA := cuda cudart curand
LD_FFMPEG := avcodec avformat avresample swscale avutil
LD_SYS := dl stdc++ pthread
LDS := $(LD_OPENCV) $(LD_FFMPEG) $(LD_CUDA) $(LD_SYS)
LDS := $(foreach lib, $(LDS), -l$(lib))

SRCS := $(shell cd $(SRCDIR) && find -name "*.cpp")
OBJS := $(patsubst %.cpp,%.o,$(SRCS))
OBJS := $(foreach item,$(OBJS),$(OBJDIR)/$(item))
CUS := $(shell cd $(SRCDIR) && find -name "*.cu")
CUOBJS := $(patsubst %.cu,%.o,$(CUS))
CUOBJS := $(foreach item,$(CUOBJS),$(OBJDIR)/$(item))
CS := $(shell cd $(SRCDIR) && find -name "*.c")
COBJS := $(patsubst %.c,%.o,$(CS))
COBJS := $(foreach item,$(COBJS),$(OBJDIR)/$(item))
OBJS := $(subst /./,/,$(OBJS))
CUOBJS := $(subst /./,/,$(CUOBJS))
COBJS := $(subst /./,/,$(COBJS))

all : $(BINDIR)/$(OUTNAME)
    $(ECHO) Compile done.

run: all
    @cd $(BINDIR) && ./$(OUTNAME);

$(BINDIR)/$(OUTNAME): $(OBJS) $(CUOBJS) $(COBJS)
    $(ECHO) Linking: $@
    @if [ ! -d $(BINDIR) ]; then mkdir $(BINDIR); fi
    @$(CC) $(CFLAGS) $(LIBS) -o $@ $^ $(LDS) $(RPATH)

$(CUOBJS) : $(OBJDIR)/%.o : $(SRCDIR)/%.cu
    @if [ ! -d $@ ]; then mkdir -p $(dir $@); fi
    $(ECHO) Compiling: $<
    @$(CUCC) $(CUFLAGS) $(INCS) -c -o $@ $<

$(OBJS) : $(OBJDIR)/%.o : $(SRCDIR)/%.cpp
    @if [ ! -d $@ ]; then mkdir -p $(dir $@); fi
    $(ECHO) Compiling: $<
    @$(CC) $(CFLAGS) $(INCS) -c -o $@ $<

$(COBJS) : $(OBJDIR)/%.o : $(SRCDIR)/%.c
    @if [ ! -d $@ ]; then mkdir -p $(dir $@); fi
    $(ECHO) Compiling: $<
    @$(CC) $(CFLAGS) $(INCS) -c -o $@ $<

clean:
    rm -rf $(OBJDIR) $(BINDIR)/$(OUTNAME)
```

- eg:

```makefile
TARGET = app

SRCS  = $(shell find ./src     -type f -name *.cpp)
HEADS = $(shell find ./include -type f -name *.h)
OBJS = $(SRCS:.cpp=.o)
DEPS = Makefile.depend

INCLUDES = -I./include
CXXFLAGS = -O2 -Wall $(INCLUDES)
LDFLAGS = -lm


all: $(TARGET)

$(TARGET): $(OBJS) $(HEADS)
    $(CXX) $(LDFLAGS) -o $@ $(OBJS)

run: all
    @./$(TARGET)

.PHONY: depend clean
depend:
    $(CXX) $(INCLUDES) -MM $(SRCS) > $(DEPS)
    @sed -i -E "s/^(.+?).o: ([^ ]+?)\1/\2\1.o: \2\1/g" $(DEPS)

clean:
    $(RM) $(OBJS) $(TARGET)

-include $(DEPS)
```

### 使用c++ 11 编译

g++ -g -Wall -std=c++11 main.cpp

### 嵌套makefile

在一些大的工程中，我们会把我们不同模块或是不同功能的源文件放在不同的目录中，我们可以在每个目录中都书写一个该目录的Makefile，这有利于让我们的Makefile变得更加地简洁，而不至于把所有的东西全部写在一个Makefile中，这样会很难维护我们的Makefile，这个技术对于我们模块编译和分段编译有着非常大的好处。

例如，我们有一个子目录叫subdir，这个目录下有个Makefile文件，来指明了这个目录下文件的编译规则。那么我们总控的Makefile可以这样书写：

```makefile
subsystem:
    cd subdir && $(MAKE)
等价于
subsystem:
    $(MAKE) -C subdir
```

定义$(MAKE)宏变量的意思是，也许我们的make需要一些参数，所以定义成一个变量比较利于维护。这两个例子的意思都是先进入“subdir”目录，然后执行make命令。

还有一个在“嵌套执行”中比较有用的参数， -w 或是 —print-directory 会在make的过程中输出一些信息，让你看到目前的工作目录。
当你使用 -C 参数来指定make下层Makefile时， -w 会被自动打开的。如果参数中有-s （ —slient ）或是 —no-print-directory ，那么， -w 总是失效的。

## Exclude source file in compilation using Makefile

从makefile编译列表中除去特别的源文件

```makefile
SRC_FILES := $(wildcard src/*.cpp)
SRC_FILES := $(filter-out src/bar.cpp, $(SRC_FILES))
```

[c++ - Exclude source file in compilation using Makefile - Stack Overflow](https://stackoverflow.com/questions/10276202/exclude-source-file-in-compilation-using-makefile)

## makefile 导出宏区分编译

字符串需要加入转义字符

```makefile
CXX_FLAGS += -DBMODEL=\"bmodel\"

#ifndef BMODEL
#enif
```