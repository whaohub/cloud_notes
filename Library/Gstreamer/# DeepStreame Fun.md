# DeepStreame Fun

## deepstream-test1

### nvds_get_user_meta_type

```
NvDsMetaType nvds_get_user_meta_type    (    gchar *     meta_descriptor    )    
generates a unique user metadata type from the given string describing user specific metadata.
从描述用户特定元数据的给定字符串生成唯一的用户元数据类型

meta_descriptor    A pointer to string describing metadata. The format of the string should be specified as below ORG_NAME.COMPONENT_NAME.METADATA_DESCRIPTION. e.g. (NVIDIA.NVINFER.TENSOR_METADATA)
```

### gst_buffer_get_nvds_batch_meta

```
NvDsBatchMeta* gst_buffer_get_nvds_batch_meta    (    GstBuffer *     buffer    )    
Gets the NvDsBatchMeta added to the GstBuffer.
获取添加到GstBUffer中的NvDsBatchMeta

Parameters
[in]    buffer    GstBuffer
Returns
A pointer to the NvDsBatchMeta structure; or NULL if no NvDsMeta was attached.
```

* cudaDeviceProp 数据结构
  
  ```
  struct cudaDeviceProp
  {
  char name[256];         //器件的名字
  size_t totalGlobalMem;    //Global Memory 的byte大小
  size_t sharedMemPerBlock;   //线程块可以使用的共用记忆体的最大值。byte为单位，多处理器上的所有线程块可以同时共用这些记忆体
  int regsPerBlock;                 //线程块可以使用的32位寄存器的最大值，多处理器上的所有线程快可以同时实用这些寄存器
  int warpSize;                    //按线程计算的wrap块大小
  size_t memPitch;        //做内存复制是可以容许的最大间距，允许通过cudaMallocPitch（）为包含记忆体区域的记忆提复制函数的最大间距，以byte为单位。
  int maxThreadsPerBlock;   //每个块中最大线程数
  int maxThreadsDim[3];       //块各维度的最大值
  int maxGridSize[3];             //Grid各维度的最大值
  size_t totalConstMem;  //常量内存的大小
  int major;            //计算能力的主代号
  int minor;            //计算能力的次要代号
  int clockRate;     //时钟频率
  size_t textureAlignment; //纹理的对齐要求
  int deviceOverlap;    //器件是否能同时执行cudaMemcpy()和器件的核心代码
  int multiProcessorCount; //设备上多处理器的数量
  int kernelExecTimeoutEnabled; //是否可以给核心代码的执行时间设置限制
  int integrated;                  //这个GPU是否是集成的
  int canMapHostMemory; //这个GPU是否可以讲主CPU上的存储映射到GPU器件的地址空间
  int computeMode;           //计算模式
  int maxTexture1D;          //一维Textures的最大维度  
  int maxTexture2D[2];      //二维Textures的最大维度
  int maxTexture3D[3];      //三维Textures的最大维度
  int maxTexture2DArray[3];     //二维Textures阵列的最大维度
  int concurrentKernels;           //GPU是否支持同时执行多个核心程序
  }
  ```
  
  # DeepStream Plugin

## NVDSANALYTICS

## deepstream 自定义元数据类型

The members you must set are user_meta_data, meta_type, copy_func, and release_func

* copy_fun
  函数在被有拷贝或在buffer中传递元数据时调用
  
  * release_fun
    元数据释放时调用

Tags:
  DeepStream, Fun, Plugin