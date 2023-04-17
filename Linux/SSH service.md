[toc]

## ssh 服务

保持活跃

```shell
ssh -o TCPKeepAlive=yes -o ServerAliveCountMax=20 -o ServerAliveInterval=15
```

**在终端命令行中输入“sudo /etc/init.d/ssh restart”命令重启ssh服务即可**。

[chroot系统中，启动sshd -d 后，不能正常登录报错Ssh, error: openpty: No such file or directory - 嗷嗷鹿鸣[VX|dshoub] - 博客园](

## SSH keys for authentication

### Preparing your server

在服务器添加ssh key ，create a hidden folder to your user account home directory on your cloud server with the following command

```shell
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

### Generate a new key pair in a terminal with the next command

```shell
ssh-keygen -t rsa
```

The key generator will ask for the location and file name to which the key is saved. Enter a new name or use the default by pressing enter.



###  (Optional) Create a passphrase for the key when prompted

This is a simple password that will protect your private key should someone be able to get their hands on it. Enter the password you wish or continue without a password. Press enter twice. Note that some automation tools might not be able to unlock passphrase-protected private keys.



### Copy the public half of the key pair to your cloud server using the following command

Replace the `user` and `server` with your username and the server address you wish to use the key authentication on. 客户端执行

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server
```

ref: [ https://upcloud.com/resources/tutorials/use-ssh-keys-authentication ]