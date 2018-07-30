使用 `yum` 安装 Nginx：

```
yum install nginx -y
```

- [**](undefined)

  ​

修改 [/etc/nginx/conf.d/default.conf](undefined)，去除对 IPv6 地址的监听**，可参考下面的示例：

[default.conf](undefined)

- [**](undefined)

  ​

修改完成后，启动 Nginx：

```
nginx
```

- [**](undefined)

  ​

此时，可访问实验机器外网 HTTP 服务（[http://119.29.147.175](undefined)）来确认是否已经安装成功。

将 Nginx 设置为开机自动启动：

```
chkconfig nginx on
```



使用 `yum` 安装 MySQL：

```
yum install mysql-server -y
```

- [**](undefined)

  ​

安装完成后，启动 MySQL 服务：

```
service mysqld restart
```

- [**](undefined)

  ​

设置 MySQL 账户 root 密码：

```
/usr/bin/mysqladmin -u root password 'MyPas$word4Word_Press'
```

- [**](undefined)

  ​

将 MySQL 设置为开机自动启动：

```
chkconfig mysqld on
```



安装 PHP

使用 `yum` 安装 PHP：**

```
yum install php-fpm php-mysql -y
```

安装之后，启动 PHP-FPM 进程：

```
service php-fpm start
```

- [**](undefined)

  ​

启动之后，可以使用下面的命令查看 PHP-FPM 进程监听哪个端口 **

```
netstat -nlpt | grep php-fpm
```

- [**](undefined)

  ​

把 PHP-FPM 也设置成开机自动启动：

```
chkconfig php-fpm on
```