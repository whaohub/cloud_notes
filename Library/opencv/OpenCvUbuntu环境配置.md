# OpenCvUbuntu环境配置

### 从源码安装opencv

1. 安装构建工具和所有依赖软件包

```
sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
```

1.  克隆所有的opencv和opencv contrib源;&#x20;

    <pre><code><strong>mkdir ~/opencv_build &#x26;&#x26; cd ~/opencv_build
    </strong>git clone https://github.com/opencv/opencv.git
    </code></pre>
2. optional&#x20;

```
git clone https://github.com/opencv/opencv_contrib.git
```

1.  下载完成后创建一个临时构建目录，并且切换到这个目录：

    ```
    cd ~/opencv_build/opencv
    mkdir -p build && cd build
    ```

    使用CMake命令配置OpenCv构建：

```
cmake -D CMAKE_BUILD_TYPE=Debug \
    -D CMAKE_INSTALL_PREFIX=/opt/opencv \
    -D INSTALL_C_EXAMPLES=ON \
    -D CMAKE_VERBOSE_MAKEFILE=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D BUILD_EXAMPLES=ON ..
```

输出成功如下

```
-- Configuring done
-- Generating done
-- Build files have been written to: /ubuntuDisk2/libary/opencv_build/opencv/build
```

1. 开始编译过程

```
make -j8   
-f 为处理器核心数， 可以输入nproc找到
```

1. 安装OpenCv:

```
sudo make install
```

1.  验证安装结果 C++ bindings:

    ```
    pkg-config --modversion opencv4
    ```

Tags: 3rdLib, OpenCv
