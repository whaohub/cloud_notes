# FFmpeg常用函数

## avformat_open_input()

FFMPEG打开媒体的的过程开始于avformat_open_input，因此该函数的重要性不可忽视。

在该函数中，FFMPEG完成了：

输入输出结构体AVIOContext的初始化；

输入数据的协议（例如RTMP，或者file）的识别（通过一套评分机制）:1判断文件名的后缀 2读取文件头的数据进行比对；

使用获得最高分的文件协议对应的URLProtocol，通过函数指针的方式，与FFMPEG连接（非专业用词）；

剩下的就是调用该URLProtocol的函数进行open,read等操作了

```c
//参数ps包含一切媒体相关的上下文结构，有它就有了一切，本函数如果打开媒体成功，  
//会返回一个AVFormatContext的实例．  
//参数filename是媒体文件名或URL．  
//参数fmt是要打开的媒体格式的操作结构，因为是读，所以是inputFormat．此处可以  
//传入一个调用者定义的inputFormat，对应命令行中的 -f xxx段，如果指定了它，  
//在打开文件中就不会探测文件的实际格式了，以它为准了．  
//参数options是对某种格式的一些操作，是为了在命令行中可以对不同的格式传入  
//特殊的操作参数而建的
int avformat_open_input(AVFormatContext **ps,  
        const char *filename,  
        AVInputFormat *fmt,  
        AVDictionary **options) 
```

[avformat_open_input](https://blog.csdn.net/leixiaohua1020/article/details/8661601)
