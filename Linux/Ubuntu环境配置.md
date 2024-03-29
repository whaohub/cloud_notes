[toc]

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

Custom rm to move files to Trash
If you accidentally remove some files on a Linux system, it will be very difficult to restore them. Therefore, it’s a safe strategy to set the rm command to move files or folders to the Trash rather than to delete them. We can empty the Trash when needed later.

This customization can be done with aliasing, which is a very handy tool on Linux (more on this later):

alias rm='gio trash'
```

## 终端使用代理

```shell
export http_proxy=http://127.0.0.1:port
如果是https那么就经过如下命令：
export https_proxy=http://127.0.0.1:port
```

https://www.cnblogs.com/ip99/p/13224767.html)

## Linux 网速测试

```shell
sudo apt install speedtest-cli
speedtest-cli --secure
```



## 查看用户当前shell环境

```shell
echo $SHELL
echo $0
```

## 用户没有sudo权限

> *** not in the sudoers file

```shell
1. 	Open sudoers file using command : sudo nano /etc/sudoers

2. 	Then add 
	"YourUsername ALL=(ALL)  ALL" 
	below the root user
	
	or
	su -
	usermod -aG sudo <username>
	exit

	Remember to reboot the virtual machine after giving yourself sudo righs 没有重启也cheng gong le
```



