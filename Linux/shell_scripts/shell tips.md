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

   [shell]:https://unix.stackexchange.com/questions/159513/what-are-the-shells-control-and-redirection-operators