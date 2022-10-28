# Ubuntu 环境配置

## 修改Ubuntu下终端输出为英文

export LC_ALL=C 所有后续命令输出均为英文。

如果要还原为本机语言，请取消设置LC_ALL变量： unset LC_AL

## 中文乱码问题

加入到bashrc 文件

```shell
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
export LANGUAGE=en_US.UTF-8
```

### 点击Dock图标最小化功能

```shell
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

## 回收站 trash-cli 命令

```shell
sudo apt install trash-cli
Trash-Cli 的使用不难，因为它提供了一个很简单的语法。Trash-Cli 提供了下面这些命令：

trash-put： 删除文件和目录（仅放入回收站中）
trash-list ：列出被删除了的文件和目录
trash-restore：从回收站中恢复文件或目录 trash.
trash-rm：删除回收站中的文件
trash-empty：清空回收站


//定期清理30天
(crontab -l ; echo "@daily $(which trash-empty) 30") | crontab -
```

## 终端使用代理

```shell
export http_proxy=http://127.0.0.1:port
如果是https那么就经过如下命令：
export https_proxy=http://127.0.0.1:port
```

## ssh 服务

保持活跃

```shell
ssh -o TCPKeepAlive=yes -o ServerAliveCountMax=20 -o ServerAliveInterval=15
```

**在终端命令行中输入“sudo /etc/init.d/ssh restart”命令重启ssh服务即可**。

[chroot系统中，启动sshd -d 后，不能正常登录报错Ssh, error: openpty: No such file or directory - 嗷嗷鹿鸣[VX|dshoub] - 博客园](https://www.cnblogs.com/ip99/p/13224767.html)

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