# Container

## Find pair by key within a vector of pairs

根据容器中保存pair的key查找element

```cpp
auto it = std::find_if( sortList.begin(), sortList.end(),
    [&User](const std::pair<std::string, int>& element){ return element.first == User.name;} );
```

## Unordered container

### unordered_set

Internally, the elements are not sorted in any particular order, but organized into buckets. Which bucket an element is placed into depends entirely on the hash of its value. This allows fast access to individual elements, since once a hash is computed, it refers to the exact bucket the element is placed into.

Rehashing invalidates iterator and might cause the elements to be re-arranged in different buckets but it doesn't invalidate references to the elements.

But, internally an implementation of unordered container might use list or other ordered container to store elements and store only references to sublists in its buckets, that would make iteration order to coincide with the insertion order until enough elements are inserted to cause list rearranging. That is the case with VS implementation.

unordered sets are optimized for insertion and retrieval, not traversal. If insertion order is your priority, use a different container.

> Different behaviour with std::unordered_map container on Windows and Linux ?

> So you are wondering why an unordered container has different orders on different implementations compiled using different compilers running on different operating systems?

lol~~~

Tags:
  Container, cpp