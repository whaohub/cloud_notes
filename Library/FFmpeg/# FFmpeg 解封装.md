# FFmpeg 解封装

## 媒体文件解析分流

1. 媒体流文件处理
   a.  正常的.mp4文件会包含两个媒体流，一个音频流，一个视频流。但是也有很多mp4文件只有音频流或者只有视频流，因此在打开解码器的时候要进行判断一下
   b.  当媒体文件打开后，需要枚举所有流信息，从而获得相应的音视频信息，从而提供给解码器使用。
2. 流媒体文件相关的API (以下都属于libavformat库中的函数)

```
      a. avformat_open_input() / avformat_close_input();
      b. avformat_seek_file()
      c. av_read_frame() 
```

## ffmpeg为AVPacket添加解码头信息

FFmpeg解码获得的AVPacket只包含视频压缩数据，并没有包含相关的解码信息
（比如：h264的sps pps头信息，AAC的adts头信息），没有这些编码头信息解
码器（MediaCodec）是识别不到不能解码的。在FFmpeg中，这些头信息是保存
在解码器上下文（AVCodecContext）的extradata中的，所以我们需要为每一种
格式的视频添加相应的解码头信息，这样解码器（MediaCodec）才能正确解析
每一个AVPacket里的视频数据。

```
const AVBitStreamFilter *absFilter = NULL;
AVBSFContext *absCtx = NULL;
AVCodecParameters *codecpar = NULL;

//1. 找到相应解码器的过滤器
if(strcasecmp(codecName, "h264") == 0){
    absFilter = av_bsf_get_by_name("h264_mp4toannexb");
}else if(strcasecmp(codecName, "h265") == 0){
    absFilter = av_bsf_get_by_name("hevc_mp4toannexb");
}

//2.过滤器分配内存
av_bsf_alloc(absFilter,absCtx)

//3. 添加解码器属性
if(pFormatCtx->streams[i]->codecpar->codec_type == AVMEDIA_TYPE_VIDEO){
    codecpar = pFormatCtx->streams[i]->codecpar;
}
avcodec_parameters_copy(absCtx->par_in, codecpar);

//4. 初始化过滤器上下文
av_bsf_init(absCtx);

//5. AVPacket处理
if(av_bsf_send_packet(absCtx, avPacket) != 0){
    av_packet_free(&avPacket);
    av_free(avPacket);
    avPacket = NULL;
    continue;
}
while(av_bsf_receive_packet(absCtx, avPacket) == 0){
    LOGE("开始解码");
    av_packet_free(&avPacket);
    av_free(avPacket);
    continue;
}
avPacket = NULL;

//6. 释放资源
av_bsf_free(&absCtx);
absCtx = NULL;
```

主要使用的类AVBitStreamFilter

## RTSP 网络流解析

使用ffmpeg播放RTSP做的一点参数优化。

先做如下定义：

AVDictionary* options = NULL;
1.画质优化

原生的ffmpeg参数在对1920x1080的RTSP流进行播放时，花屏现象很严重，根据网上查的资料，可以通过增大“buffer_size”参数来提高画质，减少花屏现象

如：

```
av_dict_set(&options, "buffer_size", "1024000", 0);
```

2.RTSP连接不上导致卡死的问题

原生的ffmpeg参数在打开RTSP流时，若连接不上， ffmpeg用avformat_open_input()解析网络流时，默认是阻塞的。当遇到解析错误的网络流时，会导致该函数长时间不返回。，可以通过设置超时来改变卡死的情况为此可以设置ffmpeg的-stimeout的参数，要注意-stimeout的单位是us微妙

如设置20s超时：

```
av_dict_set(&options, "stimeout", "20000000", 0);  //设置超时断开连接时间
```

3.其他

可以设置的参数还有很多，如可以设置连接为TCP，设置最大延时等等

```
av_dict_set(&options, "max_delay", "500000", 0);   //最大延迟
av_dict_set(&options, "rtsp_transport", "tcp", 0);  //
```

## 减少解复用时间优化

```
//输入并解析流
        LOG_PRINTF(LOG_LEVEL_DEBUG, LOG_MODULE_DECODER, "will open input file %s.", pu8FileName);

        ret = avformat_open_input(&ifmt_ctx, (char *)pu8FileName, 0, 0);
        if (ret < 0)
        {
            LOG_PRINTF(LOG_LEVEL_ERROR, LOG_MODULE_DECODER, "Could not open input file.");
            continue;
        }
        else
        {
            LOG_PRINTF(LOG_LEVEL_ERROR, LOG_MODULE_DECODER, "Could open input file %s success.", pu8FileName);
        }
        //减少流格式探测时间
        ifmt_ctx->max_analyze_duration = AV_TIME_BASE;
        ret = avformat_find_stream_info(ifmt_ctx, 0);
        if (ret < 0)
        {
            LOG_PRINTF(LOG_LEVEL_ERROR, LOG_MODULE_DECODER, "Failed to retrieve input stream information.");
            goto close_ifmt;
        }
```

ifmt_ctx->max_analyze_duration = AV_TIME_BASE;
可以有效的降低解复用的时长。

Tags:
  ffmpeg