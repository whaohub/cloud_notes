# Change Docker root directory /var/lib/docker to another location

# step1

停掉docker 服务

```shell
$ sudo systemctl stop docker.service
$ sudo systemctl stop docker.socket
```

## step2

编辑系统docker关联的系统文件 `/lib/systemd/system/docker.service` 

```shell
$ sudo vi /lib/systemd/system/docker.service
```

## step3

需要修改的行

```shell
ExecStart=/usr/bin/dockerd -H fd://
```

将要修改的行后加-g 接想要修改的新目录路径

```shell
ExecStart=/usr/bin/dockerd -g /new/path/docker -H fd://
```

## step4

创建目录

```shell
$ sudo mkdir -p /new/path/docker
```

## step5

数据同步

```shell
$ sudo rsync -aqxP /var/lib/docker/ /new/path/docker
```

## step6

reload the systemd configuration for Docker, since we made changes earlier. Then, we can start Docker.

```shell
$ sudo systemctl daemon-reload
$ sudo systemctl start docker
```

## step7

验证是否正常

```shell
$ ps aux | grep -i docker | grep -v grep
root     3919385  0.4  0.5 1753856 86084 ?       Ssl  22:19   0:04 /usr/bin/dockerd -g /home/docker_root -H fd:// --containerd=/run/containerd/containerd.sock
```

[Change Docker root directory]:https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux
