# Linux 网络服务

## firewall端口转发

开启防火墙伪装：

```shell
firewall-cmd --add-masquerade --permanent  //开启后才能转发端口 --permanent代表永久生效）
```

添加转发规则：

```shell
firewall-cmd sudo firewall-cmd --add-forward-port port=8065:proto=tcp:toport=80 --permanent
```

（PS：此规则将本机80端口转发到192.168.1.1的8080端口上，配置完--reload才生效）

重启防火墙 ：

```shell
 firewall-cmd --reload
```

如果配置完以上规则后仍不生效，检查防火墙是否开启80端口，如果80端口已开启，仍无法转发，可能是由于内核参数文件sysctl.conf未配置ip转发功能，具体配置如下：

vi /etc/sysctl.conf

在文本内容中添加：net.ipv4.ip_forward = 1

保存文件后，输入命令sysctl -p生效

[firewall-cmd]: https://www.csdn.net/tags/MtTaMg4sNjIzNTg3LWJsb2cO0O0O.html

## ufw 防火墙

```
sudo ufw enable // 开启防火墙
sudo ufw deny port   //设置限定端口
sudo ufw allow to any //开放其他所有端口
```

[ufw 官方文档]: https://help.ubuntu.com/community/UFW#Advanced_Example
[ufw]: https://www.cnblogs.com/zqifa/p/ubuntu-ufw-1.html
[iptables冲突]:https://dmesg.app/ufw-iptables.html

## tcpdump 抓包

-nnA:显示json

```shell
tcpdump -v -X -i enp3s0f0 port 1883 -nnA
```

```shell
tcpdump -i -A eth1 host 10.64.45.53 and port 8080

-A 查看详细信息
```



## nc 监听端口

****

## netstat

查看socket使用情况

```shell
 netstat -na | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```



## ss 命令

```
-h：显示帮助信息；

-V：显示指令版本信息；

-n：不解析服务名称，以数字方式显示；

-a：显示所有的套接字；

-l：显示处于监听状态的套接字；

-o：显示计时器信息；

-m：显示套接字的内存使用情况；

-p：显示使用套接字的进程信息；

-i：显示内部的TCP信息；

-4：只显示ipv4的套接字；

-6：只显示ipv6的套接字；

-t：只显示tcp套接字；

-u：只显示udp套接字；

-d：只显示DCCP套接字；

-w：仅显示RAW套接字；

-x：仅显示UNIX域套接字。
```

