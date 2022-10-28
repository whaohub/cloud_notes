# Abort Headers



[abort memory headers](https://stackoverflow.com/posts/23097862/timeline)

`<memory.h>` and `<memory>` aren't different styles, they are two completely different headers.

`<memory.h>` looks like it's an internal header used by MS's C library, you shouldn't be including it, use the standard C++ header `<memory>`.


