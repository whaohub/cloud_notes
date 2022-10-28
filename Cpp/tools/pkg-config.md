# 

**pkg-config  查看gcc的CFLAGS参数**

$ pkg-config  **--libs --cflags libavcodec**

```shell
 pkg-config  --libs --cflags libavcodec
-I/opt/ffmpeg/include -L/opt/ffmpeg/lib -lavcodec
```

**缺省情况下，** pkg-config **首 先在prefix/lib/pkgconfig/中查找相关包（譬如opencv）对应的相应的文件（opencv.pc）。在linux上上述路径名为 /usr/lib/pkconfig/。若是没有找到，它也会到PKG_CONFIG_PATH这个环境变量所指定的路径下去找。若是没有找到，它就会报 错**，例如：

Package opencv was not found in the  pkg-config search path.  
Perhaps you should add the directory containing `opencv.pc'  
to the PKG_CONFIG_PATH environment variable  
No package 'opencv' found

设置环境变量PKG_CONFIG_PATH方法举例如下：

```
export PKG_CONFIG_PATH=/home/user/ffmpeg_build/lib/pkgconfig:$PKG_CONFIG_PATH
```








