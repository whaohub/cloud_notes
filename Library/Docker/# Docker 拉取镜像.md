# Docker 拉取镜像

## 使用脚本安装docker

```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
```

## Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, Containerd, and Docker Compose packages:
   
   ```
   $ sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
   ```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:
   
   ```
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

# Docker 安装ubuntu

1. 查看Ubuntu版本
   访问Ubuntu镜像库地址:  https://hub.docker.com/_/ubuntu?tab=tags&page=1

2. 拉取镜像
   
   ```
   docker pull ubuntu
   或
   docker pull ubuntu:latest
   
   docker pull ubuntu:18.04 //特定版本
   ```

3.运行容器

```
sudo docker run --name cloudstaringleak -it --gpus all --net=host cloudstaring/full:v1.0 

docker run -itd --name ubuntu-test ubuntu  //默认最新版本

docker run -t -i ubuntu:18.04 /bin/bash  // 启动一个bash终端
```

其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开。
在交互模式下，用户可以通过所创建的终端来输入命令

# Docker 安装opencv

```
docker run --rm -it -v $(pwd):/source schickling/opencv
//Compile
 g++ $(pkg-config --cflags --libs opencv) my-file.cpp
```

Tags:
  Docker, 库框架