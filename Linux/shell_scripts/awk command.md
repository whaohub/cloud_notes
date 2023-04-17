# awk command



```shell
cat demo.txt 

   1   │ root:x:0:0:root:/root:/bin/bash
   2   │ daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

```



## awk 基本用法

```shell
# 格式
$ awk 动作 文件名

# 示例
$ awk '{print $0}' demo.txt
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

```

`demo.txt`是`awk`所要处理的文本文件。前面单引号内部有一个大括号，里面就是每一行的处理动作`print $0`。其中，`print`是打印命令，`$0`代表当前行，因此上面命令的执行结果，就是把每一行原样打印出来。

`awk`会根据空格和制表符，将每一行分成若干字段，依次用`$1`、`$2`、`$3`代表第一个字段、第二个字段、第三个字段等等

```bash
$ echo 'this is a test' | awk '{print $3}'
a
```

上面代码中，`$3`代表`this is a test`的第三个字段`a`

文件的字段分隔符是冒号（`:`），所以要用`-F`参数指定分隔符为冒号。然后，才能提取到它的第一个字段。

```bash
awk -F ':' '{ print $1 }' demo.txt 
root
daemon
```

