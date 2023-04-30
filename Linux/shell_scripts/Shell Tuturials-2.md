[toc]

# Shell Tuturials-2

## printf 命令

## Shell 输入输出重定向

大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回​​到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。

重定向命令列表如下
命令 | 说明
--- | ---
command > file        |        将输出重定向到 file。
command < file        |        将输入重定向到 file。
command >> file      |        将输出以追加的方式重定向到 file。
n > file                     |        将文件描述符为 n 的文件重定向到 file。
n >> file                    |         将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m                       |         将输出文件 m 和 n 合并。
n <& m                       |         将输入文件 m 和 n 合并。
<< tag                        |         将开始标记 tag 和结束标记 tag 之间的内容作为输入。

## for loop

for 循环一般模式

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

## while loop

```shell
#!/bin/bash

while true; do
  clear
  free -m
  sleep 1
done

```

sample: 循环打印倒计时

```shell
#!/bin/bash

# Set the countdown duration in seconds
duration=10

# Loop through the countdown
for ((i=$duration; i>=1; i--))
do
# this can be achieved using a carriage return \r:可以覆盖之前输出
  printf "%d/5\r" "$i"   # 可以覆盖之前输出
  echo "Countdown: $i seconds remaining"
  sleep 1
done

# Display the final message
echo "Countdown complete!"
```





## shell function

There are two typical ways of declaring a function. I prefer the second approach.

```bash
function function_name {
   command...
} 
```

or

```bash
function_name () {
   command...
} 
```

To call a function with arguments:

```bash
function_name "$arg1" "$arg2"
```

The function refers to passed arguments by their position (not by name), that is `$1`, `$2`, and so forth. **`$0`** is the name of the script itself.

Example:

```bash
function_name () {
   echo "Parameter #1 is $1"
}
```

## switch case statement

### Syntax

```shell
case word in
   pattern1)
      Statement(s) to be executed if pattern1 matches
      ;;
   pattern2)
      Statement(s) to be executed if pattern2 matches
      ;;
   pattern3)
      Statement(s) to be executed if pattern3 matches
      ;;
   *)
     Default condition to be executed
     ;;
esac
```

根据word 匹配到要执行的语句，匹配失败执行默认语句,;;代表跳出case 语句，类似break

A good use for a case statement is the evaluation of command line arguments as follows −

```shell
#!/bin/sh

option="${1}" 
case ${option} in 
   -f) FILE="${2}" 
      echo "File name is $FILE"
      ;; 
   -d) DIR="${2}" 
      echo "Dir name is $DIR"
      ;; 
   *)  
      echo "`basename ${0}`:usage: [-f file] | [-d directory]" 
      exit 1 # Command to come out of the program with status 1
      ;; 
esac 
```

Here is a sample run of the above program −

```shell
$./test.sh
test.sh: usage: [ -f filename ] | [ -d directory ]
```

[ref ]: https://www.tutorialspoint.com/unix/case-esac-statement.htm

[Shell Tutorials-1](simplenote://note/13b685ca-cf98-4042-a289-9ab52bfd113c)

Tags:
  linux, shell