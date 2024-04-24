# FFMPEG

\[toc]

## FFMPEG

#### FFmpeg 源码安装

1.  下载源码

    ```shell
    git clone https://github.com/FFmpeg/FFmpeg  //官网下载或使用官网镜像地址下载
    ```
2.  安装yasm汇编器

    ```shell
    sudo apt install yasm
    ```
3. 编译打开debug安装
4.  安装sdl，否则无法编译出ffplay

    ```shell
    sudo apt install libsdl2-dev
    ```
5.  一定要加--disable-stripping， 如果不加此选项，[ffmpeg](https://so.csdn.net/so/search?q=ffmpeg\&spm=1001.2101.3001.7020)在编译时，会使用strip去掉符号信息

    ```shell
    sudo ./configure --enable-sdl --enable-shared \
                    --enable-debug --disable-optimizations \
                    --disable-asm --disable-stripping \
                    --prefix=/opt/ffmpeg

    sudo make && sudo make install 
    ```
6.  config path

    ```shell
    # configure the PATH param
    echo "# Add FFMpeg bin & library paths:" >> ~/.bashrc
    echo "export PATH/opt/ffmpeg/ffmpeg/bin:$PATH" >> ~/.bashrc
    echo "export PKG_CONFIG_PATH=/opt/ffmpeg/lib/pkgconfig:$PKG_CONFIG_PATH" >> ~/.bashrc
    source ~/.bashrc
    ```

    ```
    ```

#### 一 FFMPEG 中主要库

1. libavformat: 媒体文件容器格式处理库。这个库进行码流文件解析和混流。
2. libavcodec: 编解码器库。所有的音视频编码和解码操作需要使用这个库。
3. libswresample: 音频格式转换和重采样处理的库。音频前处理、后处理就需要用到这个库。
4. libswscale: 视频格式转换和缩放处理的库。视频前处理、后处理就需要用到这个库。
5. libavfilter: 音视频滤镜、特效处理的库，例如：视频增加滤镜、音频添加音效等可以使用这个库里的函数。
6. libavdevice: 设备操作库。上面提到的要在 控件界面显示、音频输出设备摄像头控制、麦克风 等控制可以使用这个库。 在大部分的操作系统中，这个是跟平台相关的，所以在很短实际项目中，比较少使用这个库，而是根据项目情况直接调用操作系统API进行处理。
7. libavutil: Utility辅助函数库，提供一些独立的辅助函数功能

在这几个库中，前四个库比较重要，是大部分的音视频项目中都会使用到的，而且对于滤镜、特效操作，大部分项目都会根据自己需求专门开发这方面的算法，所以较少直接使用FFMPEG的这个库，设备相关操作也是大部分直接调用操作系统平台API

#### 二 FFMPEG 重点数据结构和基础

* a) 解协议(http, rtsp, rtmp,mms)

AVIOContext, URLProtocol, URLContext主要存储视音频使用的协议的类型以及状态。URLProtocol存储输 入视音频使用的封装格式。每种协议都对应一个URLProtocol结构。（注意：FFMPEG中文件也被当做一种协 议“file”

* b) 解封装(flv, avi,rmvb,mp4)

AVFormatContext主要存储视音频封装格式中包含的信息；AVInputFormat存储输入视音频使用的封装格式。每种视音频封装格式都对应一个AVInputFormat 结构.

* c) 解码(h264,mpeg2,aac,mp3)

每个AVStream存储一个视频/音频流的相关数据；每个AVStream对应一个AVCodecContext，存储该视频/音频流使用解码方式的相关数据；每个AVCodecContext中对应一个AVCodec，包含该视频/音频对应的解码器。每种解码器都对应一个AVCodec结构。

* d) 存数据

视频一般每个结构是存一帧;音频可能有好几帧 解码前数据:AVPacket 解码后数据:AVFrame

对应关系如下:

[![HqgfbT.md.jpg](https://s4.ax1x.com/2022/02/19/HqgfbT.md.jpg)](https://imgtu.com/i/HqgfbT)

* AVFormatContext AVFormatContext是包含码流参数较多的结构体,AVFormatContext是一个贯穿始终的数据结构，很多函数都要用到它作为参数。它是FFMPEG解封装（flv，mp4，rmvb，avi）功能的结构体。下面看几个主要变量的作用（在这里考虑解码的情况）

```c
AVIOContext *pb：输入数据的缓存

unsigned int nb_streams：视音频流的个数

AVStream **streams：视音频流

char filename[1024]：文件名

int64_t duration：时长（单位：微秒us，转换为秒需要除以1000000）

int bit_rate：比特率（单位bps，转换为kbps需要除以1000）

AVDictionary *metadata：元数据

```

* 编解码器信息 AVCodec是存储编解码器信息的结构体

下面说一下最主要的几个变量：

```c

const char *name：编解码器的名字，比较短

const char *long_name：编解码器的名字，全称，比较长

enum AVMediaType type：指明了类型，是视频，音频，还是字幕

enum AVCodecID id：ID，不重复

const AVRational *supported_framerates：支持的帧率（仅视频）

const enum AVPixelFormat *pix_fmts：支持的像素格式（仅视频）

const int *supported_samplerates：支持的采样率（仅音频）

const enum AVSampleFormat *sample_fmts：支持的采样格式（仅音频）

const uint64_t *channel_layouts：支持的声道数（仅音频）

int priv_data_size：私有数据的大小

```

* 音视频数据帧

AVFrame 结构体用来表示未进行编码压缩的音视频数据，其中主要的几个字段：

```c

typedef struct AVFrame
 {
  ......

  //
  // 视频帧图像数据 或者 音频帧PCM数据, 根据不同的格式有不同的存放方式
  // 对于视频帧：RGB/RGBA 格式时 data[0] 中一次存放每个像素的RGB/RGBA数据
  //            YUV420 格式时 data[0]存放Y数据;  data[1]存放U数据; data[2]存放V数据
  // 对于音频帧: data[0]存放左声道数据;  data[1]存放右声道数据
  //
  uint8_t *data[AV_NUM_DATA_POINTERS];  

  //
  // 行字节跨度, 相当于stride
  // 对于视频帧: 上下两行同一列像素相差的字节数,例如：对于RGBA通常是(width*4), 但是有时FFMPEG内部会有扩展, 可能会比这个值大
  // 对于音频帧: 单个通道中所有采样占用的字节数
  //
  int linesize[AV_NUM_DATA_POINTERS];

  int format;         // 对于视频帧是图像格式; 对于音频帧是采样格式  
  int64_t pts;        // 当前数据帧的时间戳

  int width, height;  // 仅用于视频帧, 宽度高度
  int key_frame;      // 仅用于视频, 当前是否是I帧

  int sample_rate;          // 仅用于音频, 采样率
  uint64_t channel_layout;  // 仅用于音频, 通道类型
  int nb_samples;           // 仅用于音频, 样本数量

  ......
}

```

常用的操作函数:

```c

AVFrame *av_frame_alloc(void);  // 分配一个数据帧结构

AVFrame *av_frame_clone(const AVFrame *src); // 完整的克隆数据帧结构, 包括其内部数据

void av_frame_free(AVFrame **frame);  // 释放数据帧结构及其内部数据

int av_frame_ref(AVFrame *dst, const AVFrame *src);  // 增加引用计数

void av_frame_unref(AVFrame *frame);  // 减少引用计数

```

* 音视频数据包

AVPacket 结构体用来表示压缩后的音视频数据，其中主要的几个字段：

```c

typedef struct AVPacket
 {
  ......
  int64_t pts;        // 显示时间戳   
  int64_t dts;        // 解码时间戳 (对于音频来说通常与pts相同)
  uint8_t *data;      // 实际压缩后的视频或者音频数据
  int     size;       // 压缩后的数据大小
  int     stream_index;  // 流索引值, 在媒体文件中,使用0,1来区分音视频流,可以通过这个值区分当前包是音频还是视频
  int   flags;

  int64_t duration;     // 渲染显示时长,对于视频帧比较有用,控制一帧视频显示时长
  int64_t pos;          // 当前包在流文件中的位置, -1表示未知
  ......
} AVPacket;

```

常用操作函数:

```c

AVPacket *av_packet_alloc(void);  // 分配一个数据包结构体

AVPacket *av_packet_clone(const AVPacket *src);  // 完整赋值一个数据包

void av_packet_free(AVPacket **pkt);  // 释放数据包结构及其内部的数据

void av_init_packet(AVPacket *pkt);   // 初始化数据包结构,可选字段都设置为默认值

int av_new_packet(AVPacket *pkt, int size); // 根据指定大小创建包结构中的数据

```

Tags: FFMPEG, 库框架, 音视频
