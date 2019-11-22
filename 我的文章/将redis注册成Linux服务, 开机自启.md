# 将redis注册成Linux服务, 开机自启

一.先下载解压redis，然后进入utils目录

![这里写图片描述](https://img-blog.csdn.net/20161109091753178)

二.打开文件redis_init_script

![这里写图片描述](https://img-blog.csdn.net/20161109092255462)

三.根据实际环境重新写路径，注意最后的两行蓝色注释要加上。PIDFILE先去/var/run看看有没有redis开头的pid文件，没有的话先去redis-3.2.5/src下执行 ./redis-server ../redis.conf 手动启动redis，然后pid结尾的文件就会生成。
![这里写图片描述](https://img-blog.csdn.net/20161109092158102)

四.将redis_init_script 脚本拷贝到 /etc/init.d 下 修改名字为 redis

![这里写图片描述](https://img-blog.csdn.net/20161109093214903)

![这里写图片描述](https://img-blog.csdn.net/20161109093225153)

五. chmod +x /etc/init.d/redis 修改读写权限

六.重新回来redis目录打开redis.conf,修改如下图，no改成yes

![这里写图片描述](https://img-blog.csdn.net/20161109093620514)

六.尝试启动或停止redis
service redis start
service redis stop

七. 开启服务自启动
chkconfig redis on

八.加入开机自启服务
chkconfig –add redis

大功完成！机器重启就会看到redis已经启动了
