# Gstreamer

* 官方文档： https://gstreamer.freedesktop.org/documentation/tutorials/index.html?gi-language=c
* gst-inspect-1.0 -a  查看插件信息

 `pkg-config --cflags --libs gstreamer-1.0`

```
/* Initialize GStreamer */
gst_init(&argc, &argv);
```

这必须是第一个GStreamer 命令

* 初始化所有内部结构

* 检查可用的插件

* 执行用于 GStreamer 的任何命令行选项

## Pad

pads 是元素连接外部的接口，数据流从一个元素的source pad到另一个元素的sink pad。
一个pad的类型由俩个属性定义：its direction and its availability.（方向和可用性）:

1. Gstreamer有俩个pad 方向：source pads 和sink pads。
2. pad 可以有三种可用性中的任何一种: always, sometimes, on request 
   2.1 always pads always exist.
   2.2 sometimes pads exist only in certain cases (and can disappear randomly)
   2.3 on-request pads appear only if explicitly requested by applications

### Dynamic(sometimes) pads

创建元素时，某些元素可能没有所有的焊盘。例如，这可能发生在 Ogg demuxer 元素中。当它在 Ogg 流中检测到这样的流时，该元素将读取 Ogg 流并为每个包含的基本流（vorbis、theora）创建动态填充。

## pipeline

GStreamer 中的所有元素在使用之前通常必须包含在管道中，因为它负责一些时钟和消息传递功能。我们使用gst_pipeline_new ()创建管道。

这些元素之间还没有相互联系。为此，我们需要使用gst_element_link ()。它的第一个参数是源，第二个参数是目标。顺序很重要，因为必须按照数据流（即从源元素到汇元素）建立链接。请记住，只有位于同一个 bin 中的元素才能链接在一起，因此请记住在尝试链接它们之前将它们添加到管道中

## Linking elements

通过将源元素与零个或多个类似过滤器的元素以及最终的接收器元素链接起来，您就可以设置媒体管道。数据将流经元素。这是 GStreamer 中媒体处理的基本概念。

[![5KJ8Mt.png](https://z3.ax1x.com/2021/10/13/5KJ8Mt.png)](https://imgtu.com/i/5KJ8Mt)

通过链接这三个元素，我们创建了一个非常简单的元素链。这样做的效果是源元素的输出将用作类似过滤器的元素的输入。类似过滤器的元素会对数据做一些事情并将结果发送到最终的接收器元素。

将上图想象成一个简单的 Ogg/Vorbis 音频解码器。源是从磁盘读取文件的磁盘源。第二个元素是 Ogg/Vorbis 音频解码器。sink 元素是你的声卡，播放解码后的音频数据

您必须在链接之前将元素添加到 bin 或管道，因为将元素添加到 bin 会断开任何现有的链接。此外，您不能直接链接不在同一个 bin 或管道中的元素；如果您想链接不同层次级别的元素或焊盘，则需要使用ghost pad

### Element State

创建后，元素实际上不会执行任何操作。您需要更改元素状态以使其执行某些操作。GStreamer 知道四种元素状态，每种状态都有非常特定的含义。这四种状态是：

* GST_STATE_NULL: 这是默认状态。在此状态下没有分配资源，因此，转换到该状态将释放所有资源。当元素的引用计数达到 0 并被释放时，元素必须处于此状态。

* GST_STATE_READY: 在就绪状态下，一个元素已经分配了它所有的全局资源，即可以保存在流中的资源。您可以考虑打开设备、分配缓冲区等。但是，在此状态下未打开流，因此流位置自动为零。如果之前打开了一个流，它应该在这种状态下关闭，并且应该重置位置、属性等。

* GST_STATE_PAUSED: 在这种状态下，一个元素已经打开了流，但没有主动处理它。一个元素被允许修改流的位置，读取和处理数据和状态更改为PAUSED这样的准备回放尽快，但它不是 可以玩这将使时钟运行的数据。总之，PAUSED 与 PLAYING 相同，但没有运行时钟。
  
  进入PAUSED状态的元素应该PLAYING为尽快转移到状态做好准备。例如，视频或音频输出将等待数据到达并将其排队，以便在状态更改后立即播放。此外，视频接收器已经可以播放第一帧（因为这还不会影响时钟）。Autoplugger 可以使用相同的状态转换来将管道连接在一起。但是，大多数其他元素（例如编解码器或过滤器）在此状态下不需要显式执行任何操作。

* GST_STATE_PLAYING：在PLAYING状态中，元素与状态中的完全相同PAUSED，除了时钟现在运行。

您可以使用函数更改元素的状态 gst_element_set_state ()。如果将元素设置为另一个状态，GStreamer 将在内部遍历所有中间状态。因此，如果您将元素设置为NULLto PLAYING，GStreamer 将在内部将元素设置为READY和PAUSED之间。

当移动到 时GST_STATE_PLAYING，管道将自动处理数据。它们不需要以任何形式迭代。在内部，GStreamer 将启动线程来为它们执行此任务。GStreamer 还将通过使用 GstBus. 有关详细信息，请参阅总线。

当你将一个 bin 或管道设置为某个目标状态时，它通常会自动将状态变化传播到 bin 或管道内的所有元素，因此通常只需要设置顶级管道的状态即可启动管道或关闭它。但是，当动态地向已经运行的管道添加元素时，例如从“pad- added”信号回调中，您需要使用gst_element_set_state ()或将其设置为所需的目标状态gst_element_sync_state_with_parent ()

## tee

将数据拆分到多个pad，分支数据在捕获视频时很有用，例如在屏幕上显示视频，并且需要编码写入文件。
需要在每个分支中使用单独 的queue元素来为每个分支提供单独的线程。否则一个线程中的阻塞数据流会使其他分支终止。

Tags:
  gstreamer, 库框架