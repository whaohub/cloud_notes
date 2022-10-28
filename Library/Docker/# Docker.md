# Docker

* Docker-- 从入门到实践:  https://yeasy.gitbook.io/docker_practice/
* Docker Ubuntu安装:  https://yeasy.gitbook.io/docker_practice/install/ubuntu

#### 删除本地镜像

删除本地镜像使用如下命令:

```
$ docker image rm [opation] <镜像1> [<镜像2> ...]
```

用ID，镜像名，摘要删除镜像

* 删除镜像时遇到问题 

```
Error response from daemon: conflict: unable to delete 597ce1600cf4 (must be forced) - image is being used by stopped container
```

a. 首先找到使用镜像的容器

```shell
docker ps -a

hd@hd:~ $ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND            CREATED          STATUS                      PORTS     NAMES
16c723414b8a   ubuntu         "bash"             11 minutes ago   Exited (0) 27 seconds ago             ubuntu-test
61aa0a411adc   mba2           "/usr/sbin/init"   2 weeks ago      Exited (137) 2 weeks ago              mba2
69f852dfb83c   9160de71d67b   "/usr/sbin/init"   7 weeks ago      Up 24 hours                           mba
```

b. 终止容器

```
hd@hd:~ $ sudo docker stop container（16c723414b8a）
```

c. 删除容器

```
docker rm container
```

d. 删除镜像

```
docker rmi imageId
```

### 利用commit理解镜像构成

### docker 加入时间

```shell
docker cp /etc/localtime 【容器ID或者NAME】:/etc/localtime
sudo docker cp /usr/share/zoneinfo/Asia/Shanghai 【容器ID或者NAME】:/:/etc/localtime
sudo docker restart 【容器ID或者NAME】:
```

docker 随开机启动:  docker update --restart=always 容器ID(或者容器名)

  [docker](https://so.csdn.net/so/search?q=docker&spm=1001.2101.3001.7020)  save -o xxx.tar 镜像名

## docker ssh连接没有环境变量问题

```shell
export $(cat /proc/1/environ |tr '\0' '\n' | xargs)
```
