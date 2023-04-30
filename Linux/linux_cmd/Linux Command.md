[TOC]

# Linux Command

## 基本配置

man Ubuntu汉化

```
1.sudo apt-get update # 更新你的下载源目录，此步骤可省略。
2.sudo apt-get install manpages-zh # 系统会自动下载并安装
3.vi ~/.bashrc # 编辑家目录下的bash配置文件
4.在最后一行输入：alias cman='man -M /usr/share/man/zh_CN' # 将中文的man命令重命名为cman命令，之后保存并退出编辑
5.source ~/.bashrc # 重新运行.bashrc文件

报错解决 can't set the locale; make sure $LC_* and $LANG are correct
sudo apt-get -y install language-pack-zh-hans
```

## linux 查看GPU版本和详细信息硬件命令

* Nvidia GPU使用信息查询：
  
  ```shell
  nvidia-smi
  ```

* 查看GPU型号
  
  ```shell
  lspci | grep -i nvidia
  ```

* 查看NVIDIA驱动版本
  
  ```shell
  sudo dpkg --list | grep nvidia-*
  ```

* lshw 命令
  lshw 是一个查看硬件信息工具，主要在 Linux 下使用。lshw 可以检视整体硬件情况，也可以获取某项硬件设备的详细信息。支持检测包括 BIOS，主板配置，CPU，内存，硬盘，网卡，USB/SCSI 控制器等。

* lscpu 命令 
  lscpu命令，查看的是cpu的统计信息.
  
  ```sh
  架构：           x86_64
  CPU 运行模式：   32-bit, 64-bit
  字节序：         Little Endian
  CPU:             16
  在线 CPU 列表：  0-15
  每个核的线程数： 2
  每个座的核数：   8
  座：             1
  NUMA 节点：      1
  厂商 ID：        GenuineIntel
  CPU 系列：       6
  型号：           165
  型号名称：       Intel(R) Core(TM) i7-10700 CPU @ 2.90GHz
  步进：           5
  CPU MHz：        800.157
  CPU 最大 MHz：   4800.0000
  CPU 最小 MHz：   800.0000
  BogoMIPS：       5799.77
  虚拟化：         VT-x
  L1d 缓存：       32K
  L1i 缓存：       32K
  L2 缓存：        256K
  L3 缓存：        16384K
  NUMA 节点0 CPU： 0-15
  ```

```
##  apt- 命令

```shell
/var/log/apt/history.log  //可以查看包安装日志
```

1. 列表包
   列出所有可用包

```sh
sudo apt list
//命令过滤输出
sudo apt list | grep package_name
```

2. 列出已安装的包
   
   ```shell
   sudo apt list --installed
   ```

3. 获取可升级软件包列表

```shell
sudo apt list --upgradeable
```

## export 命令

## ln 命令

err: Linux符号连接的层数过多

解决: 创建符号链接的时候一定要使用绝对路径,不要使用相对路径

### 文件信息命令

## stat 查看文件详细信息

```shell
 whao@ThinkPadX1:~/whao/Shell$ stat HelloShell.sh
  File: HelloShell.sh
  Size: 29              Blocks: 8          IO Block: 4096   regular file
Device: 820h/2080d      Inode: 95867       Links: 1
Access: (0755/-rwxr-xr-x)  Uid: ( 1000/    whao)   Gid: ( 1000/    whao)
Access: 2021-10-16 23:39:48.816422300 +0800
Modify: 2021-10-16 22:47:52.006422300 +0800
Change: 2021-10-16 22:47:52.006422300 +0800
 Birth: -
```

## wc  一般用于统计文件的信息，比如文本的行数，文件所占的字节数

```shell
 whao@ThinkPadX1:~/whao/Shell$ wc HelloShell.sh
 2  4 29 HelloShell.sh
```

统计当前目录下文件的个数（不包括目录)

```shell
$ ls -l | grep "^-" | wc -l
```

grep "^-" 
 过滤ls的输出信息，只保留一般文件，只保留目录是grep "^d"

统计当前目录下文件的个数（包括子目录）

```
 ls -lR| grep "^-" | wc -l
```

## du 命令一般用于统计文件和目录所占用的空间大小

```shell
whao@ThinkPadX1:~/whao/Shell$ du -h HelloShell.sh
4.0K    HelloShell.sh
```

* du -sh : 查看当前目录总共占的容量。而不单独列出各子项占用的容量 

* du -lh --max-depth=1 : 查看当前目录下一级子文件和子目录占用的磁盘容量。

* du -h -d 1 / | sort -hr   顺序输出空间占用大小

### ldconfig命令

 ldconfig命令的用途主要是在默认搜寻目录/lib和/usr/lib以及动态库配置文件/etc/ld.so.conf内所列的目录下，搜索出可共享的动态链接库（格式如lib*.so*）,进而创建出动态装入程序(ld.so)所需的连接和缓存文件。缓存文件默认为/etc/ld.so.cache，此文件保存已排好序的动态链接库名字列表，为了让动态链接库为系统所共享，需运行动态链接库的管理命令ldconfig，此执行程序存放在/sbin目录下。

ldconfig通常在系统启动时运行，而当用户安装了一个新的动态链接库时，就需要手工运行这个命令

## 查看相关命令

* tail 命令
  tail 命令从指定点开始将文件写到标准输出.使用tail命令的-f选项可以方便的查阅正在改变的日志文件,tail -f filename会把filename里最尾部的内容显示在屏幕上,并且不但刷新,使你看到最新的文件内容
  
  1. 命令格式
      tail [必要参数][选择参数][文件]
      2.命令功能
      用于显示指定文件末尾内容，不指定文件时，作为输入信息进行处理。常用查看日志文件。
  
  2. 命令参数
       --retry
       即使tail开始时就不能访问 或者在tail运行后不能访问,也仍然不停地尝试打开文件.  -- 只与-f合用时有 用.
       -c, --bytes=N
     
              输出最后N个字节
     
       -f, --follow[={name|descriptor}]
     
              当文件增长时,输出后续添加的数据; -f, --follow以及 --follow=descriptor 都是相同的意思
     
       -n, --lines=N
     
              输出最后N行,而非默认的最后10行
     
       --max-unchanged-stats=N
     
              参看texinfo文档(默认为5)
     
       --max-consecutive-size-changes=N
     
              参看texinfo文档(默认为200)
     
       --pid=PID
     
              与-f合用,表示在进程ID,PID死掉之后结束.
     
       -q, --quiet, --silent
     
              从不输出给出文件名的首部
     
       -s, --sleep-interval=S
     
              与-f合用,表示在每次反复的间隔休眠S秒
     
       -v, --verbose
     
              总是输出给出文件名的首部
4. 使用示例
   
   ```sh
        whao@ThinkPadX1:~$     ping 127.0.0.1 > test.log &
        whao@ThinkPadX1:~$ tail -f test.log
        PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
        64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.688 ms
        64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.054 ms
        64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.083 ms
        64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.039 ms
        64 bytes from 127.0.0.1: icmp_seq=5 ttl=64 time=0.056 ms
        64 bytes from 127.0.0.1: icmp_seq=6 ttl=64 time=0.059 ms
        64 bytes from 127.0.0.1: icmp_seq=7 ttl=64 time=0.058 ms
        64 bytes from 127.0.0.1: icmp_seq=8 ttl=64 time=0.109 ms
        64 bytes from 127.0.0.1: icmp_seq=9 ttl=64 time=0.043 ms
        64 bytes from 127.0.0.1: icmp_seq=10 ttl=64 time=0.059 ms
        64 bytes from 127.0.0.1: icmp_seq=11 ttl=64 time=0.042 ms
        64 bytes from 127.0.0.1: icmp_seq=12 ttl=64 time=0.041 ms
        64 bytes from 127.0.0.1: icmp_seq=13 ttl=64 time=0.041 ms
        64 bytes from 127.0.0.1: icmp_seq=14 ttl=64 time=0.041 ms
        64 bytes from 127.0.0.1: icmp_seq=15 ttl=64 time=0.043 ms
        64 bytes from 127.0.0.1: icmp_seq=16 ttl=64 time=0.074 ms
        64 bytes from 127.0.0.1: icmp_seq=17 ttl=64 time=0.081 ms
   ```

## head 命令

  与tail 命令相似，行为相反，查看文件开头的文字区块

## locate / whereis  命令

locate 让使用者可以很快速的搜寻档案系统内是否有指定的档案。其方法是先建立一个包括系统内所有档案名称及路径的数据库，之后当寻找时就只需查询这个数据库，而不必实际深入档案系统之中了

1. 命令格式
   locate [args][样式]
   2. 命令功能
      locate命令可以在搜寻数据库时快速找到档案，数据库由updatedb程序来更新，updatedb是由cron daemon周期性建立的，locate命令在搜寻数据库时比由整个由硬盘资料来搜寻资料来得快，但较差劲的是locate所找到的档案若是最近才建立或 刚更名的，可能会找不到，在内定值中，updatedb每天会跑一次，可以由修改crontab来更新设定值。(etc/crontab)

locate指定用在搜寻符合条件的档案，它会去储存档案与目录名称的数据库内，寻找合乎范本样式条件的档案或目录录，可以使用特殊字元（如”*” 或”?”等）来指定范本样式，如指定范本为kcpa*ner, locate会找出所有起始字串为kcpa且结尾为ner的档案或目录，如名称为kcpartner若目录录名称为kcpa_ner则会列出该目录下包括 子目录在内的所有档案。

locate指令和find找寻档案的功能类似，但locate是透过update程序将硬盘中的所有档案和目录资料先建立一个索引数据库，在 执行loacte时直接找该索引，查询速度会较快，索引数据库一般是由操作系统管理，但也可以直接下达update强迫系统立即修改索引数据库。
3.命令参数
-e   将排除在寻找的范围之外。

-1  如果 是 1．则启动安全模式。在安全模式下，使用者不会看到权限无法看到 的档案。这会始速度减慢，因为 locate 必须至实际的档案系统中取得档案的 权限资料。

-f   将特定的档案系统排除在外，例如我们没有到理要把 proc 档案系统中的档案 放在资料库中。

-q  安静模式，不会显示任何错误讯息。

-n 至多显示 n个输出。

-r 使用正规运算式 做寻找的条件。

-o 指定资料库存的名称。

-d 指定资料库的路径

-h 显示辅助讯息

-V 显示程式的版本讯息

## 删除命令

* rm删除除了指定文件的其他所有文件

```sh
rm -v !("filename"|"filename")
```

* 删除指定类型文件

删除当前目录下的所有类型的文件，命令（注意指定的目录，否则会递归删除掉子目录中文件）
find . -type f -delete或find . -type f -exec rm -f {} \; 

rm-f `find 指定目录 -type f`

- 通过inode使用find删除中文乱码的文件
  
  linux下有一些文件比较特别，无法直接删除或者容易误删除成其他文件。删除这类文件时，可以不通过文件名，可以通过inode号进行删除
  
  使用ls -i 查看inode 
  
  ```shell
  root@master:/home/workspace/zs_videoevent_app/arm_lib# ll -i
  total 130536
  60429945 drwxr-xr-x.  6 root root      4096 Sep  6 14:57 ./
  71303425 drwxr-xr-x. 10 root root      4096 Sep  6 14:57 ../
  71317805 -rw-r--r--.  1 root root 105338015 Sep  6 14:47 0902.zip
  60429946 -rw-r--r--.  1 root root     14616 Oct 29  2021 libRtmpPushModelByZlm.so
  60429947 -rw-r--r--.  1 root root   1444080 Oct 29  2021 libevent.so
  ```
  
  ```shell
  find . -inum 71317805 -exec rm -i {} \;
  ```
  
  为避免删错文件，这里使用了rm的交互式删除。不需要交互时，可以将rm后的-i 去掉，也可以直接使用delete进行删除：
  
  ```shell
  find . -inum 271761664 -delete
  ```

## find command

### Some frequently used Cleanup commands

**delete empty directories**
`find /path/to/dir -empty -type d -delete    `

**delete all recursive subdir particular file type:**
`find . -type f -iname "*.zip" -delete`

**delete files older than 30 days:**
`find ~ -type f ! -atime 30|xargs ls -lrt`

**find largest 10 files in a directory:(**[**link**](https://www.cyberciti.biz/faq/how-do-i-find-the-largest-filesdirectories-on-a-linuxunixbsd-filesystem/)**)**
`for i in G M Kdodu -ah | grep [0-9]$i | sort -nr -k 1done | head -n 11`

https://www.tecmint.com/35-practical-examples-of-linux-find-command/

> 递归删除文件夹下指定名字文件

```shell
find ./ -name *.log | xargs rm -f
```

## kill 命令 问题

有下面两种情况进程是无法杀掉的：

一、进程已经成为僵尸进程，此时需要等待其父进程回收或者kill掉其父进程即可。

二、进程正处于内核态度中。我们知道Linux进程运行时分为内核态和用户态两种，当进程进入内核态时，会屏蔽所有信号，包括SIGKILL，因此kill -9 也会变得无效。

## ps 命令查看进程启动及运行时间

```sh
ps -eo lstart 启动时间
ps -eo etime   运行多长时间.
ps -eo pid,lstart,etime | grep
```

## history 加入显示时间

```sh
export HISTTIMEFORMAT="%F %T " 
```

## linux 解压命令

* 压缩
  
  ```shell
  tar -cvf jpg.tar *.jpg //将目录里所有jpg文件打包成tar.jpg
  
  tar -czf jpg.tar.gz *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
  
  tar -cjf jpg.tar.bz2 *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
  
  tar -cZf jpg.tar.Z *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
  
  rar a jpg.rar *.jpg //rar格式的压缩，需要先下载rar for linux
  
  zip jpg.zip *.jpg //zip格式的压缩，需要先下载zip for linux
  ```

* 解压

```shell
tar -zxvf file.tgz --strip-components 1   //解压并去除第一层级
tar -xvf file.tar //解压 tar包

tar -xzvf file.tar.gz //解压tar.gz

tar -xjvf file.tar.bz2 //解压 tar.bz2

tar -xZvf file.tar.Z //解压tar.Z

tar -Jxvf fle.tar.xz  //解压tar.xz

unrar x file.rar   // 按原目录结构解压rar文件

unzip -O CP936 filename.zip   //解压zip出现乱码时需要加的参数
```

> 解压遇到问题
> 
> tar: Cannot connect to  ....
> 
> <u>添加</u>--force-local 解压参数或 6
> 
> (https://unix.stackexchange.com/posts/13379/timeline)
> 
> When you download with `wget`, specify the output file name with the `-O` option.
> 
> ```bash
> wget "http://domain.com/file.tgz?crazy=args&stuff=todelete" -O file.tgz
> ```

## Linux crontab 设置定时任务

Linux **crontab** 是用来定期执行程序的命令。当安装完成操作系统之后，默认便会启动此任务调度命令。

**crond** 命令每分钟会定期检查是否有要执行的工作，如果有要执行的工作便会自动执行该工作。

**注意：**新创建的 cron 任务，不会马上执行，至少要过 2 分钟后才可以，当然你可以重启 cron 来马上执行。

而 linux 任务调度的工作主要分为以下两类：

- 1、系统执行的工作：系统周期性所要执行的工作，如备份系统数据、清理缓存

- 2、个人执行的工作：某个用户定期要做的工作，例如每隔 10 分钟检查邮件服务器是否有新信，这些工作可由每个用户自行设置

Sytax:

```
crontab [ -u user ] file
```

**说明：**

crontab 是用来让使用者在固定时间或固定间隔执行程序之用，换句话说，也就是类似使用者的时程表。

-u user 是指设定指定 user 的时程表，这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。

**参数说明**：

- -e : 执行文字编辑器来设定时程表，内定的文字编辑器是 VI，如果你想用别的文字编辑器，则请先设定 VISUAL 环境变数来指定使用那个文字编辑器(比如说 setenv VISUAL joe)

- -r : 删除目前的时程表

- -l : 列出目前的时程表

时间格式如下：

```
f1 f2 f3 f4 f5 program
```

- 其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。
- 当 f1 为 * 时表示每分钟都要执行 program，f2 为 * 时表示每小时都要执行程序，其馀类推
- 当 f1 为 a-b 时表示从第 a 分钟到第 b 分钟这段时间内要执行，f2 为 a-b 时表示从第 a 到第 b 小时都要执行，其馀类推
- 当 f1 为 */n 时表示每 n 分钟个时间间隔执行一次，f2 为 */n 表示每 n 小时个时间间隔执行一次，其馀类推
- 当 f1 为 a, b, c,... 时表示第 a, b, c,... 分钟要执行，f2 为 a, b, c,... 时表示第 a, b, c...个小时要执行，其馀类推

## 程序被killed

输出最近killed的信息

```shell
dmesg | egrep -i -B100 'killed process'
```

批量kill进程

```shell
ps aux | grep soffice | grep -v grep | awk '{print $2}' | sudo xargs kill -9
```

使用`grep -v grep`来排除`grep`自身进程； 
使用`awk '{print $2}'`获取进程信息的第二列，也就是杀死`kill`进程需要用到的`PID`； 
使用`sudo xargs kill -9`用超管权限将前面获取到的`PID`进程杀死；

## 编码转换iconv 命令

## rsync 命令

[samples]:https://linuxconfig.org/rsync-command-examples
[ totural]: https://everythinglinux.org/rsync/

## lsof

查看系统里占用fd最多的进程  
用root用户运行下面的命令，可以打印出每个进程占用的fd数量（从大到小):

```shell
lsof -n | awk '{print $2}' | sort | uniq -c | sort -nr | more
```

## 查看文件描述符数量

```shell
#查看所有进程允许打开的最大 fd 数量
 cat /proc/sys/fs/file-max
174139

#查看所有进程已经打开的 fd 数量以及允许的最大数量
cat /proc/sys/fs/file-nr
11040   0       174139

#查看单个进程允许打开的最大 fd 数量.
ulimit -n
32768

查询某个进程已经开启的文件句柄
lsof -p 进程pid | wc -l
查看所有进程各自打开的文件数
lsof -n|awk ‘{print $2}’|sort|uniq -c|sort -nr|more
```

## dstat 命令

**dstat命令** 是一个用来替换vmstat、iostat、netstat、nfsstat和ifstat这些命令的工具，是一个全能系统信息统计工具。与sysstat相比，dstat拥有一个彩色的界面，在手动观察性能状况时，数据比较显眼容易观察；而且dstat支持即时刷新，譬如输入`dstat 3`即每三秒收集一次，但最新的数据都会每秒刷新显示。和sysstat相同的是，dstat也可以收集指定的性能资源，譬如`dstat -c`即显示CPU的使用情况。

### 语法

```shell
dstat [-afv] [options..] [delay [count]]
```

### 常用选项

```shell
-c：显示CPU系统占用，用户占用，空闲，等待，中断，软件中断等信息。
-C：当有多个CPU时候，此参数可按需分别显示cpu状态，例：-C 0,1 是显示cpu0和cpu1的信息。
-d：显示磁盘读写数据大小。
-D hda,total：include hda and total。
-n：显示网络状态。
-N eth1,total：有多块网卡时，指定要显示的网卡。
-l：显示系统负载情况。
-m：显示内存使用情况。
-g：显示页面使用情况。
-p：显示进程状态。
-s：显示交换分区使用情况。
-S：类似D/N。
-r：I/O请求情况。
-y：系统状态。
--ipc：显示ipc消息队列，信号等信息。
--socket：用来显示tcp udp端口状态。
-a：此为默认选项，等同于-cdngy。
-v：等同于 -pmgdsc -D total。
--output 文件：此选项也比较有用，可以把状态信息以csv的格式重定向到指定的文件中，以便日后查看。例：dstat --output /root/dstat.csv & 此时让程序默默的在后台运行并把结果输出到/root/dstat.csv文件中。
```

如想监控swap，process，sockets，filesystem并显示监控的时间：

```shell
[root@iZ23uulau1tZ ~] dstat -tsp --socket --fs
```



### Linux使用hdparm命令来测试SSD硬盘性能

> 说明：使用hdparm可以测试SSD硬盘性能，数据准确。

## 1、安装

```javascript
yum install hdparm   #centos
apt-get install hdparm   #debian,ubuntu
```

复制

## 2、使用 测试磁盘读取速度

```shell
hdparm -t /dev/xvda
```

## 测试磁盘写速度

```shell
time dd if=/dev/zero of=/test.dbf bs=8k count=300000
```





### Linux stress命令详解

stress 命令主要用来模拟系统负载较高时的场景

https://cloud.tencent.com/developer/article/1791641
