# Coredump

## core 文件

1. 使用ulimit 查看core 文件的最大大小，若结果为 0，则表示不会生成 core文件。 
   
   2. 使用 ulimit -c FILESIZE 命令，可以限制 core 文件的大小（FILESIZE 单位为 KB）。如果生成的信息超过此大小，将会被截断，最终生成一个不完整的 core 文件。在调试此 core 文件的时候，gdb 会提示错误
   3. 使用 ulimit -c unlimited，则表示 core 文件的大小不受限制。
      在终端通过命令ulimit -c unlimited只是临时修改，重启后无效 ，要想永久修改有三种方式：
   
   ```
   （1）在/etc/rc.local 中增加一行 ulimit -c unlimited；
   （2）在/etc/profile 中增加一行 ulimit -c unlimited；
   （3）在/etc/security/limits.conf 最后增加如下两行记录：
   ```

@root soft core unlimited
@root hard core unlimited

## 调试core文件

（1）启动gdb，进入core文件，命令格式：gdb [exec file] [core file]。 
  用法示例：gdb ./test test.core。

（2）在进入gdb后，查找段错误位置：where或者bt 

Tags:
  coredump