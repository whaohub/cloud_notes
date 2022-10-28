# shell scripts tips

#### shell脚本调试

检查是否有语法错误`-n`：

```shell
bash -n script_name.sh
```

使用下面的命令来执行并调试 Shell 脚本`-x`：

```shell
bash -x script_name.sh
```

## shell脚本命令顺序执行

1. 使用分号分割
   
   ```shell
     command1 ; command2
   ```

2. 使用&&
   
   ```
    command1 && command2
   ```

   3. $? 获取上一个命令的退出状态

所谓退出状态，就是上一个命令执行后的返回结果。退出状态是一个数字，一般情况下，大部分命令执行成功会返回 0，失败返回 1，这和C语言的 main() 函数是类似的。

```shell
if [ $? -eq 0 ]
then
cmd
fi
```

   [shell]:https://unix.stackexchange.com/questions/159513/what-are-the-shells-control-and-redirection-operators