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

## docker 镜像重命名

```docker
docker image tag #imageId myname/server:latest
```



## docker批量删除

删除所有容器

> docker rm `docker ps -a -q`

删除所有镜像

> docker rmi `docker images -q`

按条件删除镜像
      删除悬浮镜像

> docker image prune

删除不提醒

> docker image prune -f


镜像名包含关键字

> docker rmi --force `docker images | grep zoo-plus | awk '{print $3}'` 
> 1.
> 其中zoo-plus为关键字

docker 启动所有的容器

> docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)

docker 关闭所有的容器

> docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)

docker 删除所有的容器

> docker rm $(docker ps -a | awk '{ print $1}' | tail -n +2)

docker 删除所有的镜像

> docker rmi $(docker images | awk '{print $3}' |tail -n +2)
> 

