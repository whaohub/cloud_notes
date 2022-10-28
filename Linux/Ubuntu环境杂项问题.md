# Ubuntu 环境杂项问题

#### Linux 问题 Note

* 由于/boot上的磁盘空间不足，无法升级
    解决： https://ubuntuqa.com/article/471.html
    !!! 使用了sudo apt-get autoemove.

* dpkg: 处理软件包 initramfs-tools (--configure)时出错：
  子进程 已安装 post-installation 脚本 返回错误状态 1
   在处理时有错误发生：
   initramfs-tools
   E: Sub-process /usr/bin/dpkg returned an error code (1)
  解决方法：
  
  ```shell
  1.$ sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old                                      //现将info文件夹更名
  ```

2.$ sudo mkdir /var/lib/dpkg/info                                                                      //再新建一个新的info文件夹

3.$ sudo apt-get update

4.apt-get -f install                                                                                            //不用解释了吧

5.$ sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old                                          //执行完上一步操作后会在新的info文件夹下生成一些文件，现将这些文件全部移到info_old文件夹下

6.$ sudo rm -rf /var/lib/dpkg/info                                                                 //把自己新建的info文件夹删掉

7.$ sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info                                   //把以前的info文件夹重新改回名字

```
## 解决每次进入终端都需要重新输入 source ~/.bashrc 问题

linux配置文件执行顺序为：
/etc/profile→ (~/.bash_profile | ~/.bash_login | ~/.profile)→ ~/.bashrc →/etc/bashrc → ~/.bash_logout

在bash_profile 加入以下脚本
```

if [ -f ~/.bashrc ] ; then
        source ~/.bashrc
fi 

```
* Linux 终端光标显示隐藏

```shell
隐藏光标 ：  echo -e "\033[?25l"  
显示光标 ：  echo -e "\033[?25h"
```

## Ubuntu 磁盘挂载

1. 安装gparted
   获取uuid信息

```
$ sudo apt-get install gparted

修改挂载
sudo gedit /etc/fstab

挂载格式
·UUID=************] [挂载磁盘分区]  [挂载磁盘格式]  0  2

//挂载到home下系统被恢复了初始化

UUID=ea219978-ef0c-4c59-9e7e-34170f7eacc2 /ubuntuDisk  ext4 defaults 0 2
```

## /etc/fstab内容详解

/etc/fstab内容主要包括六列：（通过空格或 Tab 分隔）

<file system>    <mount point>    <type>    <options>    <dump>    <pass>

第一列：设备名或者设备卷标名

第二列：设备挂载目录（/、home、sys等）

第三列：设备文件系统（ext4、ntfs、iso9660、swap 及 auto等）

第四列：挂载参数（auto、exec、ro、rw、user、users、owner、sync、async、dev、suid、noatime、relatime、flush、defaults等等）

第五列：指明是否要备份，（0为不备份，1为要备份，一般根分区要备份）

第六列：指明自检顺序。 （0为不自检，1或者2为要自检，如果是根分区要设为1，其他分区只能是2）。因此要使ubuntu不开机自检，只需将该列的值修改为0即可。

/etc/fstab就是在开机引导的时候自动挂载到linux的文件系统，磁盘被手动挂载之后都必须把挂载信息写入/etc/fstab这个文件中，否则下次开机启动时仍然需要重新挂载。

如果给计算机配了一块新磁盘，已经分区，格式化，挂载，但当计算机重启后，然后我们想让计算机启动时自动挂载，方法就是文件/etc/fstab下增加一行

系统开机时会主动读取/etc/fstab这个文件中的内容，根据文件里面的配置挂载磁盘。

## Ubuntu 系统引导主题设置

```shell
 mv theme /boot/grub/themes
```

## Ubuntu 安装微信

* deep wine
  
  ```shell
  wget -O- https://deepin-wine.i-m.dev/setup.sh | sh
  sudo apt-get install com.qq.weixin.deepin
  ```
  
  修改wine 中微信字体大小，找到wineconfig 文件，目前我的路径为

```shell
/opt/deepin-wine6-stable/bin

env WINEPREFIX="$HOME/.deepinwine/Deepin-WeChat" ./winecfg 
```

* uos 微信安装
  
  ```
  sudo dpkg -i wechat-uos_2.0.0-2_amd64.deb
  ```

// 如果报错，提示相关依赖没有装，则执行
sudo apt install -f

  wget  -O ~/weixin.deb "http://archive.ubuntukylin.com/software/pool/partner/weixin_2.1.1_amd64.deb"

    sudo dpkg -i ~/weixin.deb

```
* oh-my-zsh 国内下载源地址

1. 下载
```

sh -c "$(wget -O- https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)"

```
2. 自动补全插件
```

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

```
##  用TLP降低发热
```

sudo add-apt-repository ppa:linrunner/tlp
sudo apt update
sudo apt install tlp tlp-rdw
sudo tlp start

```

Tags:
  errornote, Linux, softwere