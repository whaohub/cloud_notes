# Linuxlog

```shell
=> /var/log/messages：常规日志消息
=> /var/log/boot：系统启动日志
=> /var/log/debug：调试日志消息
=> /var/log/auth.log：用户登录和身份验证日志
=> /var/log/daemon.log：运行squid，ntpd等其他日志消息到这个文件
=> /var/log/dmesg：Linux内核环缓存日志
=> /var/log/dpkg.log：所有二进制包日志都包括程序包安装和其他信息
=> /var/log/faillog：用户登录日志文件失败
=> /var/log/kern.log：内核日志文件
=> /var/log/lpr.log：打印机日志文件
=> /var/log/mail.*：所有邮件服务器消息日志文件
=> /var/log/mysql.*：MySQL服务器日志文件
=> /var/log/user.log：所有用户级日志
=> /var/log/xorg.0.log：X.org日志文件
=> /var/log/apache2/*：Apache Web服务器日志文件目录
=> /var/log/lighttpd/*：Lighttpd Web服务器日志文件目录
=> /var/log/fsck/*：fsck命令日志
=> /var/log/apport.log：应用程序崩溃报告/日志文件
=> /var/log/syslog：系统日志
=> /var/log/ufw：ufw防火墙日志
=> /var/log/gufw：gufw防火墙日志

#使用tail，more，less和grep命令。
tail -f /var/log/apport.log
more /var/log/xorg.0.log
cat /var/log/mysql.err
less /var/log/messages
grep -i fail /var/log/boot
```

高版本ubuntu开启message日志:高版本[ubuntu](https://so.csdn.net/so/search?q=ubuntu\&spm=1001.2101.3001.7020)系统默认没有 `/var/log/messages`，因为在 `/etc/rsyslog.d/50-default.conf` 文件中，将其注释掉了。

```shell
#*.=info;*.=notice;*.=warn;\
#        auth,authpriv.none;\
#       cron,daemon.none;\
#        mail,news.none          -/var/log/messages
```

然后重启rsyslog服务即可。

```shell
systemctl restart rsyslog.service
```

## linux 日志分析

日志默认存放位置：/var/log/

查看日志配置情况：more /etc/rsyslog.conf

| 日志文件             | 说明                                                                                |
| ---------------- | --------------------------------------------------------------------------------- |
| /var/log/cron    | 记录了系统定时任务相关的日志                                                                    |
| /var/log/cups    | 记录打印信息的日志                                                                         |
| /var/log/dmesg   | 记录了系统在开机时内核自检的信息，也可以使用dmesg命令直接查看内核自检信息                                           |
| /var/log/mailog  | 记录邮件信息                                                                            |
| /var/log/message | 记录系统重要信息的日志。这个日志文件中会记录Linux系统的绝大多数重要信息，如果系统出现问题时，首先要检查的就应该是这个日志文件                 |
| /var/log/btmp    | 记录错误登录日志，这个文件是二进制文件，不能直接vi查看，而要使用lastb命令查看                                        |
| /var/log/lastlog | 记录系统中所有用户最后一次登录时间的日志，这个文件是二进制文件，不能直接vi，而要使用lastlog命令查看                            |
| /var/log/wtmp    | 永久记录所有用户的登录、注销信息，同时记录系统的启动、重启、关机事件。同样这个文件也是一个二进制文件，不能直接vi，而需要使用last命令来查看          |
| /var/log/utmp    | 记录当前已经登录的用户信息，这个文件会随着用户的登录和注销不断变化，只记录当前登录用户的信息。同样这个文件不能直接vi，而要使用w,who,users等命令来查询 |
| /var/log/secure  | 记录验证和授权方面的信息，只要涉及账号和密码的程序都会记录，比如SSH登录，su切换用户，sudo授权，甚至添加用户和修改用户密码都会记录在这个日志文件中     |

比较重要的几个日志： 登录失败记录：/var/log/btmp //lastb 最后一次登录：/var/log/lastlog //lastlog 登录成功记录: /var/log/wtmp //last 登录日志记录：/var/log/secure

​ 目前登录用户信息：/var/run/utmp //w、who、users
