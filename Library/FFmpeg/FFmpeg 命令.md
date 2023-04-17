# FFmpeg 命令

## ffmpeg 命令

* flv 转 mp4
  由于我的flv文件使用的视频编码是h264,音频是aac，所以转码的过程中flv->mp4,仅仅是容器改变了，编码方式几乎没有变化

```
ffmpeg -i input.flv -vcodec copy -acodec copy output.
```

此过程需要对视频进行重新编码，耗费资源和cpu较为严重

```
ffmpeg -i input.flv output.mp4 
```

## ffmpeg 指定编码器编码

* h265
  -c:a复制参数告诉ffmpeg将音频流从原始文件直接复制到输出文件中。而-c:v libx265告诉ffmpeg在H中编码新的视频文件.

```
fmpeg -i inputname -c:a copy -c:v libx265 video-h265.mp4
```

* h264 convert to mpeg4

```
ffmpeg -i input.avi -c:v libxvid output.avi
```

* 本地文件推rtmp
  
  ```
  ffmpeg -re -i localFile.mp4 -c copy -f flv rtmp://server/live/streamName
  ```

* rtmp 流保存到本地文件
  
  ```
  ffmpeg  -rtsp_transport tcp  -re(视频原始帧率) -an(audio not无声) -i "rtmp://server/live/streamName" -c copy dump.flv
  ```
  
  [ref] https://blog.csdn.net/leixiaohua1020/article/details/84483279

* 保存rtsp
  
  ```shell
  ffmpeg -rtsp_transport tcp -an -re -i <rtsp_url> -c copy -f segment -segment_time 10 stream_piece_%d.mp4
  ```
  
  

* 读取制定文件图片

```
ffmpeg -i "rtsp://:8554/" -y -f image2 -r 1/1 img%03d.jpg
```

- 解码toyuv

```shell
ffmpeg -i video.mp4 -c:v rawvideo -pix_fmt yuv420p out.yuv
```



## ffmpeg 统计视频总帧数

```
ffprobe -select_streams v -show_streams input.avi
nb_frames=159697
```

## ffmpeg 剪切视频

```
ffmpeg -ss 00:03:00 -i video.mp4 -t 60 -c copy cut.mp4
```

-ss后面指定的时间轴，-t后面指定时长单位为秒。为什么要将-ss放在-i前面？因为官方文档推荐这样做，这样做剪辑出来的视频时间轴更精准，并且速度更快。还有一个参数-to放在-i video.mp4后面，作用是指定剪辑时长，例如-to 00:02:00，当-ss放在-i前面的时候，这个-to剪辑出来的是-ss指定的时间轴加上-to指定的时间，比如-ss 00:01:00 -i video.mp4 -to 00:02:00，则剪辑出来的视频，是原视频00:01:00到00:03:00的片段。如果想把片头给去掉则指定了时间轴就不要添加-to和-t参数。

## ffmpeg 分离裸流

```
ffmpeg -i h264.mp4 -c:v copy -bsf:v h264_mp4toannexb -an out.h264
```

## 合并视频

创建文件，文件中填写要合并的视频

```shell
cat fileList.txt
file '/home/file1.mp4'
file '/home/file1.mp4' 

ffmpeg -f concat -safe 0 -i fileList.txt -c copy mergedVideo.mp4
```

## delete audio

```shell
ffmpeg -i $input_file -c copy -an $output_file
```





## ffmpeg slow down /fast video

```she
ffmpeg -i input.mkv -filter:v "setpts=0.5*PTS" output.mkv  //double speed
//To slow down your video, you have to use a multiplier greater than 1:
ffmpeg -i input.mkv -filter:v "setpts=2.0*PTS" output.mkv  //slow down
```

## FFplay

播放yuv

```shell
ffplay -i output.yuv -pixel_format yu420p -s 2448x2048
ffplay -f rawvideo -video_size 1920x1080 a.yuv
```

## FFmpeg 日志

ffmpeg 命令输出日志

```shell
FFREPORT=file=ffreport.log:level=32 ffmpeg -i ....
```

- ffreport.log : 生成的日志文件
- level: 日志级别(32:info)

## ffmpeg check stream

you can use `ffprobe`:

```php
ffprobe -v quiet -print_format json -show_streams rtmp://example.com/stream
```

You'll get a return code `1` if the command failed or `0` and a JSON string containing the detected streams on success:

```json
{
    "index": 1,
    "codec_name": "aac",
    "codec_long_name": "AAC (Advanced Audio Coding)",
    "profile": "LC",
    "codec_type": "audio",
    ...
}
```

ref: https://stackoverflow.com/questions/31451716/how-to-check-rtmp-live-stream-is-on-or-off

Tags:
  FFmpeg