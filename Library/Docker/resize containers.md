# docker 容器扩容

docker 被意外停止后无法启动，docker容器被占满，需要动态扩容(最好使用宿主机目录做映射，做好数据持久化)

```shell
[whao@master dataa]$ sudo docker start 14b44
Error response from daemon: Failed to mount; dmesg: <4>[9114985.413956] XFS (dm-8): last sector read failed
<6>[9115750.875704] attempt to access beyond end of device
<6>[9115750.875706] dm-8: rw=32, want=104857600, limit=62914560
<4>[9115750.875708] XFS (dm-8): last sector read failed
: mount /dev/mapper/docker-253:0-1075256807-6f8baeafad11e75f7aa09e7711d44c377db4998f85849bb2d1e00513259b28c0:/var/lib/docker/devicemapper/mnt/6f8baeafad11e75f7aa09e7711d44c377db4998f85849bb2d1e00513259b28c0, data: nouuid: input/output error
```

```shell
#!/bin/bash

bmid=$1
space=$2

if [ "$bmid" != "" ] && [ "$space" != "" ]; then
    DEV=`docker inspect $bmid |grep DeviceName |awk -F'"' '{print $4}'`
    dmsetup table $DEV | sed "s/0 [0-9]* thin/0 $(($space*1024*1024*1024/512)) thin/" | dmsetup load $DEV;
    dmsetup resume $DEV;
    xfs_growfs /dev/mapper/$DEV;
  echo "resize $bmid success."
else
  echo "rerun";
fi
```

>  文件系统为xfs  , 系统为centos

```shell
[whao@master dataa]$ df -hT
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  7.8G     0  7.8G   0% /dev
tmpfs                   tmpfs     7.8G     0  7.8G   0% /dev/shm
tmpfs                   tmpfs     7.8G  257M  7.5G   4% /run
tmpfs                   tmpfs     7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs       1.0T  165G  860G  17% /
/dev/sda1               xfs       494M  305M  190M  62% /boot
/dev/mapper/centos-home xfs       823G  766G   57G  94% /home
```

[Docker容器硬盘热扩容操作记录 - 散尽浮华 - 博客园](https://www.cnblogs.com/kevingrace/p/6667063.html)

我是执行脚本一次性成功，没有出现无法启动情况
