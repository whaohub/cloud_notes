[toc]

# FFmpeg AVfilter

## AVfilter 库介绍

libavfilter。该类库提供了各种视音频过滤器。

## 使用流程

AVFilter的初始化比较复杂，而使用起来比较简单。初始化的时候需要avfilter_graph_config()一系列函数。而使用的时候只有两个函数：av_buffersrc_add_frame()用于向FilterGraph中加入一个AVFrame，而av_buffersink_get_frame()用于从FilterGraph中取出一个AVFrame。



ref: https://www.cnblogs.com/renhui/p/14663071.html