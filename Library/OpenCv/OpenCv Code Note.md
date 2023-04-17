# OpenCv  Code Note

## OpenCv  Tutorials Day1

day1 入门练习

```
编译选项
`pkg-config --cflags --libs opencv4`
```

### Mat类

   Mat 类是存储和操作 OpenCV 中图像的主要数据结构。这个类是在 core 模块中定义的。OpenCV 已经实现了对于这些数据结构自动分配和释放内存的机制。但是，当数据结构共享相同的缓冲存储器时，程序员仍然应该特别注意。

   例如，赋值运算符并没有从一个对象（Mat A）到另一个对象（Mat B）复制内存内容；而只是对其引用（相应内容的存储地址）的复制。之后，一个对象（A 或 B）的改变对两个对象都有影响。为了复制一个 Mat 对象的内存内容，应该使用成员函数 Mat::clone()。

   注意，OpenCV 中的许多函数在处理密集的单通道或多通道数组时，常使用 Mat 类。但是在某些场合，使用一个不同的数据类型可能很方便。例如，std::vector<>、Matx<>、Vec<> 或 Scalar。为此，OpenCV 提供了代理类 InputArray 和 OutputArray，允许前面的任意类型作为函数的参数使用。

   Mat 类用于密集的 n 维单通道或多通道数组。实际上它可以存储实数或复数值向量和矩阵、彩色图像或灰度图像、直方图、点云等。

* 图片保存到base64
  
  ```
  std::vector<uchar> buf;
  
  cv::imencode(".jpg",*dsexample->cvmat, buf);
  ```

auto *enc_msg = reinterpret_cast<unsigned char*>(buf.data());

encode = g_base64_encode(enc_msg, buf.size());

```cpp
## Cv::imwrite

```c++
cv::imwrite("./test/" + to_string(line.id)+"_"+ to_string(obj.m_uiObjId)+".jpg",mat);
```

## Conversion from IplImage* to cv::MAT

```cpp
IplImage * ipl = ...;
cv::Mat m = cv::cvarrToMat(ipl);  // default additional arguments: don't copy data.

or
Mat(const IplImage* img, bool copyData=false);
```

## unsigned char * to mat

```cpp
unsigned char * output;//image data`
cv::Mat TempMat = cv::Mat(h, w, CV_8UC1, output);
```





Tags:
  opencv