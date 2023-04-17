[toc]

# Shell Tutorials-1

## Hello Shell

HelloShell.sh

```shell
1 #!/bin/bash
2 echo Hello Shell
```

#! 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序
运行shell脚本有俩种方法：
1.作为可执行程序

```shell
chmod +x HelloShell.sh
./HelloShell.sh
```

2.作为解释器参数

```shell
/bin/sh HelloShell.sh
```

## Shell变量

* 定义变量
  
  ```shell
  name="shell"
  ```
  
  *变量名和等号之间不能有空格*

* 引用变量
  
  ```shell
  echo $name
  echo ${name}
  ```
  
  变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界,推荐给所有变量加上花括号，这是个好的编程习惯

已定义的变量可以重新定义

```
name="redefineName"
```

第二次赋值的时候不能写$name="redefineName"，使用变量的时候才加美元符（$）

* 只读变量
  使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变

*删除变量

```shell-session
unset variable_name
```

* 变量类型
  运行shell时，会同时存在三种变量：
1) 局部变量 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2) 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3) shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## Shell 字符串

字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。

* 单引号字符串的限制：

单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。
双引号的优点：

* 双引号里可以有变量
  双引号里可以出现转义字符
  
  ```shell
  str='this is a string'
  echo ${str}
  ```

value="value"
str1="hello this is a $value \n"
echo -e $str1

greeting="hello "$value""
greeting1="hello ${value}"
echo $greeting $greeting1

greeting2='hello ! '$value''
greeting3='hello ${value}'
echo $greeting2 $greeting3

echo "获取字符串长度 ${#greeting}"
echo "提取子字符串 ${greeting:1:4}" # 从第2个字符串开始截取4个字符
echo "查找字符串h或o的位置:" `expr index "$greeting" ho` # 哪个先出现就先计算哪个

```shell
##  Shell 传递参数

可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数

```shell
echo "执行脚本传入的参数为:"
echo "执行的文件名:$0"
echo "传入的第一个参数:$1"
echo "传入的第二个参数:$2"
echo "传入脚本的参数个数:$#"
echo "传入脚本的参数以字符串形式输出:$*"
echo "传入脚本的参数以字符串的形式输出:$@"
echo "脚本运行的当前id号:$$"
echo "显示最后命令的退出状态.0表示没有错误,其他任何值表明有错误 $?"

echo "\$* 与 \$@ 区别演示"
echo "-- \$* 演示 --"

for i in "$*";do
    echo $i
done


echo "\$* 与 \$@ 区别演示"
echo "-- \$@ 演示 --"


for i in "$@";do
    echo $i
done
```

## Shell 数组

Bash Shell 只支持一维数组（不支持多维数组），初始化时不需要定义数组大小（与 PHP 类似）。

与大部分编程语言类似，数组元素的下标由 0 开始。

Shell 数组用括号来表示，元素用"空格"符号分割开.

```shell
#!/bin/bash

my_array=("value0" "value1" "value3")

#也可以是用下标方式定义
array_name[0]="name0"
array_name[1]="name1"

#读取数组
echo "this is the first array name = :${array_name[0]}"
echo "this is the first my_array val=: ${my_array[0]}"

#读取所有元素
echo "my_array: ${my_array[@]}"
echo "array_name: ${array_name[*]}"

#读取数组元素个数
echo "my_array size= ${#my_array[@]}"
echo "array_name size= ${#array_name[*]}"
```

## echo 命令

```shell
!/bin/bash

#1.显示普通字符串 也可加双引号
echo this is echo test

#2.显示转义字符
echo \" this escape test \"

#3.显示变量
read name
echo "$name this is read test"

#4.显示换行  -e 开启转义
echo -e " line feed \n"
echo "this line test"

#5.显示不换行 
echo -e " not change line \c"
echo "this not change line test"

#6.显示结果重定向到文件
echo "this redirect test" > test.txt

#7.原样输出字符串，不进行转义 使用单引号
echo '$name \"'

#8.显示执行命令 使用反引号
echo `date`
```

| 参数处理 | 说明                              |
|:---- |:------------------------------- |
| $#   | 传递到脚本或函数的参数个数                   |
| $*   | 以一个单字符串显示所有向脚本传递的参数             |
| $$   | 脚本运行的当前进程ID号                    |
| $!   | 后台运行的最后一个进程的ID号                 |
| $@   | 与$*相同，但是使用时加引号，并在引号中返回每个参数。     |
| $-   | 显示Shell使用的当前选项，与set命令功能相同。      |
| $?   | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

### Reference

[Shell Tuturials-2](simplenote://note/83bb1d79-ccdd-4a8e-aeca-d9986533a445)    

[ShellTuturials](https://zhuanlan.zhihu.com/p/264346586)

[zsh 与bash 脚本的区别] (https://segmentfault.com/a/1190000011122024)

[zsh 和bash 脚本区别二](https://www.shangmayuan.com/a/60eca86f64924568877f8c7f.html    )

Tags:
  Linux, Shell