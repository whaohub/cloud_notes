#### nfs server 配置

[How to Install and Configure an NFS Server on Ubuntu 22.04](https://linuxhint.com/install-and-configure-nfs-server-ubuntu-22-04/)

> showmount – show mount information for an NFS server

showmount常用法:

```shell
showmount     (没有参数，列出所有挂载了共享目录的客户端client)

showmount –a  (列出server上共享的目录，同时列出client上的挂载点)

showmount –d  (列出被client挂载的目录)

showmount –e （列出server端的共享目录）
```

通常可以这样查询某个Server：

查询共享目录：`showmount –e serverIP(or serverName)`

查询被哪些客户端挂载：`showmount –a`

查看所有： `showmount`



##  [nfs 挂载服务器挂掉，客户端卡住问题

NFS服务端意外断开，导致挂载的客户端“df -T”命令无法使用，及挂载目录无法"cd"、"ls"等命令

该文件夹中有一个共享目录挂载在该文件夹某一目录下，因突然关机等异常情况导致该服务无限制等待

当NFS服务器启动以后依然不会恢复，只有重启系统才可恢复。

解决思路：

1，出现此类问题原因是因为客户端无法重试连服务端。

2，强制取消挂载并重新挂载。

处理方法：

1，先查看目录 mount -l 列出挂载的目录

2，强制卸载目录 umount -f -l 挂载的目录



## sshfs 配置挂载远程

```
$ sudo apt-get install sshfs     【基于 Debian/Ubuntu 的系统】
```

#### 步骤 2：创建 SSHFS 挂载目录

当你安装 SSHFS 包之后，你需要创建一个挂载点目录，在这儿你将要挂载你的远程文件系统。例如，我们在 /mnt/tecmint 下创建挂载目录。

```
# mkdir /mnt/tecmint$ sudo mkdir /mnt/tecmint     【基于 Debian/Ubuntu 的系统】
```

#### 步骤 3：使用 SSHFS 挂载远程的文件系统

当你已经创建你的挂载点目录之后，现在使用 root 用户运行下面的命令行，在 /mnt/tecmint 目录下挂载远程的文件系统。视你的情况挂载目录可以是任何目录。

下面的命令行将会在本地的 /mnt/tecmint 目录下挂载一个叫远程的一个 /home/tecmint 目录。（不要忘了使用你的 IP 地址和挂载点替换 x.x.x.x）。

```
# sshfs tecmint@x.x.x.x:/home/tecmint/ /mnt/tecmint$ sudo sshfs -o allow_other tecmint@x.x.x.x:/home/tecmint/ /mnt/tecmint      【基于 Debian/Ubuntu 的系统】
```

如果你的 Linux 服务器配置为基于 SSH 密钥授权，那么你将需要使用如下所示的命令行指定你的公共密钥的路径。

```
# sshfs -o IdentityFile=~/.ssh/id_rsa tecmint@x.x.x.x:/home/tecmint/ /mnt/tecmint$ sudo sshfs -o allow_other,IdentityFile=~/.ssh/id_rsa tecmint@x.x.x.x:/home/tecmint/ /mnt/tecmint     【基于 Debian/Ubuntu 的系统】
```