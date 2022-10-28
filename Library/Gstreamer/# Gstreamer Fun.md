# Gstreamer Fun

* gst_parse_launch(args ...)

```
GstElement *
gst_parse_launch (const gchar * pipeline_description,GError ** error)
Parameters:

pipeline_description – the command line describing the pipeline
error – the error message in case of an erroneous pipeline.
```

* gst_element_get_bus
  
  ```
  Returns the bus of the element. Note that only a GstPipeline will provide a bus for the application.
  ```

Parameters:

element – a GstElement to get the bus of.

```
* gst_element_get_static_pad 
```

GstPad * gst_element_get_static_pad (GstElement * element,const gchar * name)

根据element取出一个name的存在pad
element – a GstElement to find a static pad of.   需要查找pad的元素
name – the name of the static GstPad to retrieve   取回的pad名字

```
* gst_pad_add_probe
```

gulong gst_pad_add_probe (GstPad * pad,
                   GstPadProbeType mask,
                   GstPadProbeCallback callback,
                   gpointer user_data,
                   GDestroyNotify destroy_data)

Parameters:
pad – the GstPad to add the probe to
mask – the probe mask
callback – GstPadProbeCallback that will be called with notifications of the pad state
user_data ( [closure]) – user data passed to the callback
destroy_data – GDestroyNotify for user_data

Returns – 
an id or 0 if no probe is pending. The id can be used to remove the probe with gst_pad_remove_probe. When using GST_PAD_PROBE_TYPE_IDLE it can happen that the probe can be run immediately and if the probe returns GST_PAD_PROBE_REMOVE this functions returns 0. MT safe.

```
匹配mask的探测类型时回调用回调函数

# dst-example 实现插件时例子中的gst函数

* gst_element_register（） 

创建一个能够实例化该类型对象的新 elementfactory 并将工厂添加到plugin
为了创建名字event_judge 元素的插件
```

参数：

plugin ( [允许无] ) – GstPlugin用于注册元素，或NULL用于静态元素。
name —— 此类型元素的名称
rank —— 元素的等级（更高的等级意味着在自动插入时更重要）
type —— 要注册的元素的 GType

return :  —— TRUE , 如果注册成功, FALSE错误

```

Tags:
  Fun, gstreamer