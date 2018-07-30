### `MySQLdb` 是python2版本的库

### `mysql-connector`和`pymysql`是python3的库

#### mysal-connector的安装有以下几种

### 一、下载库文件

1.在https://pypi.python.org/pypi中搜索mysql下载

 ![TIM截图20171115201340](C:\Users\Administrator\Pictures\TIM截图20171115201340.png)

2.打开cmd命令行，将目录切换到刚刚下载的文件的目录 `cd c://.....`,`......`表示文件的目录，然后执行`python setup.py install`,其实你打开下载的文件你会发现里面有个文件叫做`setup.py`,所以前面的命令就是执行这个文件来自动安装

### 二、用python的pip工具进行安装

打开cmd命令行，执行`pip install mysql-connector`,然后会自动下载安装完成

**但是这种方法不一定会成功**，我就失败了很多次

### 三、下载库的安装包

在https://dev.mysql.com/downloads/connector/python/下载你电脑对应的版本

不要弄错32位和64位的安装包，而且python3.6的安装包没有，只有python2和python3.5之前的安装包，最新版本的小伙伴需要使用前两种方法，下载完成后点击安装就可以了

