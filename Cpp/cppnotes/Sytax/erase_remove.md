# Erase–remove惯用法

一个常见的编程任务是从[集合](https://zh.wikipedia.org/wiki/集合_(计算机科学))（collection）中删除等于某个值或满足某个标准的所有元素。C++语言可以通过手写循环完成这个任务。但更好的办法是使用C++标准模板库中的算法来实现。[[1\]](https://zh.wikipedia.org/wiki/Erase–remove惯用法#cite_note-EffSTL-1)[[2\]](https://zh.wikipedia.org/wiki/Erase–remove惯用法#cite_note-CS-2)[[3\]](https://zh.wikipedia.org/wiki/Erase–remove惯用法#cite_note-DDJ-3)

`erase`用于从一个集合中删除一个元素，但是对于基于数组的容器，如`vector`，存储在被删除元素后的所有元素都需要向前移动以避免集合中有一个空位（gap）。在同一容器中多次调用产生了大量移动元素的开销。

`algorithm`库提供了`remove`与`remove_if`算法。由于这些算法运行在两个前向迭代器确定的元素范围上，它们没有底层容器或集合的具体知识。[[1\]](https://zh.wikipedia.org/wiki/Erase–remove惯用法#cite_note-EffSTL-1)[[4\]](https://zh.wikipedia.org/wiki/Erase–remove惯用法#cite_note-Josuttis-4)这些算法并不从容器删除元素，而是把不“符合”删除标准的元素搬移到容器的前部，并保持这些元素的相对次序。该算法一次通过数据范围即可实现该目标。

由于没有元素被删除，因此容器尺寸保持不变。容器尾部的元素都是需要被删除的，但其状态未指定（unspecified state）。`remove`返回一个迭代器指向尾部这些需要用`erase`删除的元素的第一个。

同样的事情（删除多个元素），用容器的方法`erase`会导致多次遍历这个容器，每一次遍历时，在被删除元素之后的所有元素都必须向前移动，其时间消耗远大于单次通过。

```cpp
c.erase(remove(c.begin(), c.end(), 1963),c.end());
// 当c是vector、string 或deque时，
// erase-remove惯用法是去除特定值的元素的最佳方法
```

