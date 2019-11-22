# [***LINUX添加PHP环境变量：CentOS下将php和mysql命令加入到环境变量中](https://www.cnblogs.com/kenshinobiy/p/5886046.html)



 CentOS系统下如何将PHP和mysql命令加入到环境变量中，在Linux CentOS系统上 安装完php和MySQL后，为了使用方便，需要将php和mysql命令加到系统命令中，如果在没有添加到环境变量之前，执行“php -v”命令查看当前php版本信息时时，则会提示命令不存在的错误，下面我们详细介绍一下在linux下将php和mysql加入到环境变量中的方法（假 设php和mysql分别安装在/usr/local/webserver/php/和/usr/local/webserver/mysql/中）。

```
方法一：直接运行命令export PATH=$PATH:/usr/local/webserver/php/bin 和 export PATH=$PATH:/usr/local/webserver/mysql/bin
```

使用这种方法，只会对当前会话有效，也就是说每当登出或注销系统以后，PATH 设置就会失效，只是临时生效。

```
方法二：执行vi ~/.bash_profile修改文件中PATH一行，将/usr/local/webserver/php/bin 和 /usr/local/webserver/mysql/bin 加入到PATH=$PATH:$HOME/bin一行之后
```

这种方法只对当前登录用户生效

方法三：修改/etc/profile文件使其永久性生效，并对所有系统用户生效，在文件末尾加上如下两行代码

```
PATH=$PATH:/usr/local/webserver/php/bin:/usr/local/webserver/mysql/bin
export PATH
```

最后：执行 命令source /etc/profile或 执行点命令 ./profile使其修改生效，执行完可通过echo $PATH命令查看是否添加成功。

 

推荐使用第三种方法*************************

 

 我用的是lampp，PHP的执行文件放在

```
`/opt/lampp/bin/`  `目录中<br>所以在添加环境变量的时候，添加这个目录即可<br><br>用``vi``编辑profile文件的时候：<br>``vi` `/ect/profile`   `<br>然后 i 键进入编辑模式<br>输入2行之后<br>CTRL + C,输入  :wq 保存退出<br>再`
```

 

 

```bash
`Welcome to aliyun Elastic Compute Service!` `[root@iZ25b4ffkfaZ ~]``# ls``[root@iZ25b4ffkfaZ ~]``# cd /``[root@iZ25b4ffkfaZ /]``# ls``bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  sbin  selinux  srv  sys  tmp  usr  var``[root@iZ25b4ffkfaZ /]``# gedit /etc/profile``-``bash``: gedit: ``command` `not found``[root@iZ25b4ffkfaZ /]``# vi //etc/profile``[root@iZ25b4ffkfaZ /]``# vi //etc/profile``[root@iZ25b4ffkfaZ /]``# source /etc/profile``[root@iZ25b4ffkfaZ /]``# echo $PATH``/usr/local/sbin``:``/usr/local/bin``:``/sbin``:``/bin``:``/usr/sbin``:``/usr/bin``:``/root/bin``:``/opt/lampp/bin/php``[root@iZ25b4ffkfaZ /]``# php /opt/lampp/htdocs/www/orths/index.php cli timer message "wanglaing"``-``bash``: php: ``command` `not found``[root@iZ25b4ffkfaZ /]``# cd /opt/lampp/bin/php``-``bash``: ``cd``: ``/opt/lampp/bin/php``: Not a directory``[root@iZ25b4ffkfaZ /]``# vi /etc/profile``[root@iZ25b4ffkfaZ /]``# source /etc/profile``[root@iZ25b4ffkfaZ /]``# echo $PATH``/usr/local/sbin``:``/usr/local/bin``:``/sbin``:``/bin``:``/usr/sbin``:``/usr/bin``:``/root/bin``:``/opt/lampp/bin/php``:``/opt/lampp/bin``[root@iZ25b4ffkfaZ /]``# php /opt/lampp/htdocs/www/orths/index.php cli timer message "wanglaing"``Hello wanglaing!``[root@iZ25b4ffkfaZ /]``# `
```

