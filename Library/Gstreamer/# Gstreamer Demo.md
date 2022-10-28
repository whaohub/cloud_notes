# Gstreamer Demo

=====
简单过程

* gst_init()
* 创建元素（element）
* add bin(pipeline)  添加到bin/pipeline
* linking element

## Basic tutorial

### Hello world!

* basic-tutorial-1.c 文件

```
GstElement *
gst_parse_launch (const gchar * pipeline_description,GError ** error)
Parameters:

pipeline_description – the command line describing the pipeline
error – the error message in case of an erroneous pipeline.

在 GStreamer 中，您通常通过手动组装单个元素来构建管道，但是，当管道足够简单并且您不需要任何高级功能时，您可以使用快捷方式：gst_parse_launch()
```

playbin:
Playbin 为音频和/或视频播放器提供了一个独立的一体式抽象。

Playbin 可以处理音频和视频文件和功能

* 自动文件类型识别，并基于自动选择和使用正确的音频/视频/字幕解复用器/解码器
* 音频文件的可视化
* 视频文件的字幕支持。字幕可以存储在外部文件中。
* 不同视频/音频/字幕流之间的流选择
* 元信息（标签）提取
* 轻松访问最后一个视频样本
* 通过网络播放流时进行缓冲
* 带静音选项的音量控制

##Bastic -2

## Element

## Create a Element

创建元素最简单的方式是使用  gst_element_factory_make()， 此函数为新创建的元素采用工厂名称和元素名称。
当您不再需要该元素时，您需要使用 gst_object_unref (). 这会将元素的引用计数减少 1。一个元素在创建时的引用计数为 1。当引用计数减少到 0 时，元素将被完全销毁。

```
#include <gst/gst.h>

int main(int argc, char **argv)
{
    GstElement *element;

    /* init */
    gst_init(&argc, &argv);

    /* create element */
    element = gst_element_factory_make("fakesrc", "source");

    if(!element)
    {
        g_print("element create failed\n");
        return -1;
    }

    gst_object_unref(GST_OBJECT(element));

    return 0;
}
```

gst_element_factory_make实际上是两个函数组合的简写。甲 GstElement 对象是从一个工厂创建的。要创建元素，您必须GstElementFactory 使用唯一的工厂名称访问 对象。这是通过 gst_element_factory_find ().

## First Appliation

描述了一个简单 GStreamer 应用程序的所有方面，包括初始化库、创建元素、将元素打包到管道中以及播放该管道。通过执行所有这些操作，您将能够构建一个简单的 Ogg/Vorbis 音频播放器。

```
/*
 * @Author: Whao 
 * @Date: 2021-10-12 11:23:48 
 * @Last Modified by: Whao
 * @Last Modified time: 2021-10-12 21:25:02
 */
#include <gst/gst.h>
#include <glib.h>

static gboolean
bus_call (GstBus     *bus,
          GstMessage *msg,
          gpointer    data)
{
  GMainLoop *loop = (GMainLoop *) data;

  switch (GST_MESSAGE_TYPE (msg)) {

    case GST_MESSAGE_EOS:
      g_print ("End of stream\n");
      g_main_loop_quit (loop);
      break;

    case GST_MESSAGE_ERROR: {
      gchar  *debug;
      GError *error;

      gst_message_parse_error (msg, &error, &debug);
      g_free (debug);

      g_printerr ("Error: %s\n", error->message);
      g_error_free (error);

      g_main_loop_quit (loop);
      break;
    }
    default:
      break;
  }

  return TRUE;
}


static void
on_pad_added (GstElement *element,
              GstPad     *pad,
              gpointer    data)
{
  GstPad *sinkpad;
  GstElement *decoder = (GstElement *) data;

  /* We can now link this pad with the vorbis-decoder sink pad */
  g_print ("Dynamic pad created, linking demuxer/decoder\n");

  sinkpad = gst_element_get_static_pad (decoder, "sink");

  gst_pad_link (pad, sinkpad);

  gst_object_unref (sinkpad);
}

int main(int argc, char **argv)
{
    GMainLoop *loop;

    GstElement *pipeline;
    GstElement *source;
    GstElement *demuxer;
    GstElement *decoder;
    GstElement *conv;
    GstElement *sink;
    GstBus *bus;
    guint bus_watch_id;

    //init
    gst_init(&argc, &argv);

    loop = g_main_loop_new(NULL, FALSE);

    //check input args
    if(argc != 2)
    {
        g_printerr("Usage : %s<Ogg/filename>\n", argv[0]);
        return -1;
    }

    //create gstreamer elements
    pipeline = gst_pipeline_new ("audio-player");
    source   = gst_element_factory_make ("filesrc",        "file-source");
    demuxer  = gst_element_factory_make ("oggdemux",       "ogg-demuxer");
    decoder  = gst_element_factory_make ("vorbisdec",      "vorbis-decoder");
    conv     = gst_element_factory_make ("audioconvert",   "converter");
    sink     = gst_element_factory_make ("autoaudiosink",  "audio-output");


    if(!pipeline || !source || !demuxer || !decoder || !conv || !sink)
    {
        g_printerr("One element count not be created. exiting\n");
        return -1;
    }

    /* set up pipeline */

    /* set the input filename to the source element */
    g_object_set(G_OBJECT(source), "location", argv[1], NULL);

    /* add a message handler */
    bus = gst_pipeline_get_bus(GST_PIPELINE(pipeline));
    bus_watch_id = gst_bus_add_watch(bus, bus_call, loop);
    gst_object_unref(bus);

    /* add all elements into pipeline */
    /* file-source | ogg-demuxer | vorbis-decoder | converter | alsa-output */

    gst_bin_add_many(GST_BIN(pipeline), source, demuxer, decoder, conv, sink, NULL);

    /* link the elements together */
    /* file-source -> ogg-demuxer -> vorbis-decoder -> converter -> alsa-output */
    gst_element_link(source, demuxer);
    gst_element_link_many(decoder, conv, sink, NULL);
    g_signal_connect(demuxer, "pad-added", G_CALLBACK(on_pad_added), decoder);

    /* set the pipeline to playing state*/

    g_print("Now playing: %s\n", argv[1]);
    gst_element_set_state(pipeline, GST_STATE_PLAYING);

    /* iterate */
    g_print("Runing...\n");
    g_main_loop_run(loop);

     /* Out of the main loop, clean up nicely */
    g_print ("Returned, stopping playback\n");
    gst_element_set_state (pipeline, GST_STATE_NULL);

    g_print ("Deleting pipeline\n");
    gst_object_unref (GST_OBJECT (pipeline));
    g_source_remove (bus_watch_id);
    g_main_loop_unref (loop);
    return 0;
}
```

Tags:
  gstreamer