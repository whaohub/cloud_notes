[toc]

# linux 查看目标文件符号表

## nm command

nm 命令可以列出目标文件的符号，如果没有将目标文件设为参数，nm将假设文件为a.out

- 符号类型：如果是小写则符号是本地的，如果是大写，则符号是全局符号
  
  ```shell
  "N"
  
  The symbol is a debugging symbol.
  
  "p"
  
  The symbols is in a stack unwind section.
  
  "R"
  
  "r"
  
  The symbol is in a read only data section.
  
  "S"
  
  "s"
  
  The symbol is in an uninitialized data section for small objects.
  
  "T"
  
  "t"
  
  The symbol is in the text (code) section.
  "U"
  
  The symbol is undefined.
  "u"
  
  The symbol is a unique global symbol. This is a GNU extension to the standard set of ELF symbol bindings. For such a symbol the dynamic linker will make sure that in the entire process there is just one symbol with this name and type in use.
  
  "V"
  
  "v"
  
  The symbol is a weak object. When a weak defined symbol is linked with a normal defined symbol, the normal defined symbol is used with no error. When a weak undefined symbol is linked and the symbol is not defined, the value of the weak symbol becomes zero with no error. On some systems, uppercase indicates that a default value has been specified.
  
  "W"
  
  "w"
  
  The symbol is a weak symbol that has not been specifically tagged as a weak object symbol. When a weak defined symbol is linked with a normal defined symbol, the normal defined symbol is used with no error. When a weak undefined symbol is linked and the symbol is not defined, the value of the symbol is determined in a system-specific manner without error. On some systems, uppercase indicates that a default value has been specified.
  
  "-"
  
  The symbol is a stabs symbol in an a.out object file. In this case, the next values printed are the stabs other field, the stabs desc field, and the stab type. Stabs symbols are used to hold debugging information.
  
  "?"
  
  The symbol type is unknown, or object file format specific.
  ```

## Options

以下选项全名和缩写是等价的

```shell
-A
-o

--print-file-name  
Precede each symbol by the name of the input file (or archive member) in which it was found, rather than identifying the input file once only, before all of its symbols. 所有符号前打印出文件名称
```

```shell
nm -o libCodecByNvidia.so | head 
libCodecByNvidia.so:                 U avcodec_alloc_context3
libCodecByNvidia.so:                 U avcodec_decode_video2
```

```shell
-a
--debug-syms
Display all symbols, even debugger-only symbols; normally these are not listed.
-B
The same as --format=bsd (for compatibility with the MIPS nm).

-C

--demangle[=style]
Decode (demangle) low-level symbol names into user-level names. Besides removing any initial underscore prepended by the system, this makes C ++ function names readable. Different compilers have different mangling styles. The optional demangling style argument can be used to choose an appropriate demangling style for your compiler.
--no-demangle
Do not demangle low-level symbol names. This is the default.
-D
--dynamic
Display the dynamic symbols rather than the normal symbols. This is only meaningful for dynamic objects, such as certain types of shared libraries.
-f format
--format=format
Use the output format format, which can be "bsd", "sysv", or "posix". The default is "bsd". Only the first character of format is significant; it can be either upper or lower case.
-g
--extern-only
Display only external symbols.
--plugin name
Load the plugin called name to add support for extra target types. This option is only available if the toolchain has been built with plugin support enabled.
-l
--line-numbers
For each symbol, use debugging information to try to find a filename and line number. For a defined symbol, look for the line number of the address of the symbol. For an undefined symbol, look for the line number of a relocation entry which refers to the symbol. If line number information can be found, print it after the other symbol information.
-n
-v

--numeric-sort
Sort symbols numerically by their addresses, rather than alphabetically by their names.
-p
--no-sort
Do not bother to sort the symbols in any order; print them in the order encountered.
-P
--portability
Use the POSIX .2 standard output format instead of the default format. Equivalent to -f posix.
-S
--print-size
Print both value and size of defined symbols for the "bsd" output style. This option has no effect for object formats that do not record symbol sizes, unless --size-sort is also used in which case a calculated size is displayed.
-s
--print-armap
When listing symbols from archive members, include the index: a mapping (stored in the archive by ar or ranlib) of which modules contain definitions for which names.
-r
--reverse-sort
Reverse the order of the sort (whether numeric or alphabetic); let the last come first.
--size-sort
Sort symbols by size. The size is computed as the difference between the value of the symbol and the value of the symbol with the next higher value. If the "bsd" output format is used the size of the symbol is printed, rather than the value, and -S must be used in order both size and value to be printed.
--special-syms
Display symbols which have a target-specific special meaning. These symbols are usually used by the target for some special processing and are not normally helpful when included included in the normal symbol lists. For example for ARM targets this option would skip the mapping symbols used to mark transitions between ARM code, THUMB code and data.
-t radix
--radix=radix
Use radix as the radix for printing the symbol values. It must be d for decimal, o for octal, or x for hexadecimal.
--target=bfdname
Specify an object code format other than your system's default format.
-u
--undefined-only
Display only undefined symbols (those external to each object file).
--defined-only
Display only defined symbols for each object file.
-V
--version
Show the version number of nm and exit.
-X
This option is ignored for compatibility with the AIX version of nm. It takes one parameter which must be the string 32_64. The default mode of AIX nm corresponds to -X 32, which is not supported by GNU nm.

--help
Show a summary of the options to nm and exit.
```



## 查看共享对象是否包含debug段

使用objdump查看是否包含debug段

```shell
objdump -h a.out | grep .debug_info
```

使用readelf 查看

```shell
readelf -S <binary> | grep .debug
```



ref: https://linux.die.net/man/1/nm