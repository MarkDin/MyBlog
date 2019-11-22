### Linux学习

#### 目录结构介绍

Linux采用的是树状目录结构,最上层目录是根目录`/`,然后再其下面创建其他目录

在Linux中,一切皆文件



总结:

1. Linux只有一个根目录`/`
2. Linux的各个目录的存放内容都是规划好的,不能乱放文件
3. Linux是以文件的形式管理设备,因此Linux系统中一切皆为文件

###### 常用目录:

| 目录       |                        应放置档案内容                        |
| ---------- | :----------------------------------------------------------: |
| /bin       | 系统有很多放置执行档的目录，但/bin比较特殊。因为/bin放置的是在单人维护模式下还能够被操作的指令。在/bin底下的指令可以被root与一般帐号所使用，主要有：cat,chmod(修改权限), chown, date, mv, mkdir, cp, bash等等常用的指令。 |
| /boot      | 主要放置开机会使用到的档案，包括Linux核心档案以及开机选单与开机所需设定档等等。Linux kernel常用的档名为：vmlinuz ，如果使用的是grub这个开机管理程式，则还会存在/boot/grub/这个目录。 |
| /dev       | 在Linux系统上，任何装置与周边设备都是以档案的型态存在于这个目录当中。 只要通过存取这个目录下的某个档案，就等于存取某个装置。比要重要的档案有/dev/null, /dev/zero, /dev/tty , /dev/lp*, / dev/hd*, /dev/sd*等等 |
| /etc       | 系统主要的**设定档**几乎都放置在这个目录内，例如人员的帐号密码档、各种服务的启始档等等。 一般来说，这个目录下的各档案属性是可以让一般使用者查阅的，但是只有root有权力修改。 FHS建议不要放置可执行档(binary)在这个目录中。 比较重要的档案有：/etc/inittab, /etc/init.d/, /etc/modprobe.conf, /etc/X11/, /etc/fstab, /etc/sysconfig/等等。 另外，其下重要的目录有：/etc/init.d/ ：所有服务的预设启动script都是放在这里的，例如要启动或者关闭iptables的话： /etc/init.d/iptables start、/etc/init.d/ iptables stop/etc/xinetd.d/ ：这就是所谓的super daemon管理的各项服务的设定档目录。 /etc/X11/ ：与X Window有关的各种设定档都在这里，尤其是xorg.conf或XF86Config这两个X Server的设定档。 |
| /home      | 这是系统预设的使用者家目录(home directory)。 在你新增一个一般使用者帐号时，预设的使用者家目录都会规范到这里来。比较重要的是，家目录有两种代号： ~ ：代表当前使用者的家目录，而 ~guest：则代表用户名为guest的家目录。 |
| /lib       | 系统的函式库非常的多，而/lib放置的则是在开机时会用到的函式库，以及在/bin或/sbin底下的指令会呼叫的函式库而已 。 什么是函式库呢？妳可以将他想成是外挂，某些指令必须要有这些外挂才能够顺利完成程式的执行之意。 尤其重要的是/lib/modules/这个目录，因为该目录会放置核心相关的模组(驱动程式)。 |
| /media     | media是媒体的英文，顾名思义，这个/media底下放置的就是可移除的装置。 包括软碟、光碟、DVD等等装置都暂时挂载于此。 常见的档名有：/media/floppy, /media/cdrom等等。 |
| /mnt       | 如果你想要暂时挂载某些额外的装置，一般建议妳可以放置到这个目录中。在古早时候，这个目录的用途与/media相同啦。 只是有了/media之后，这个目录就用来暂时挂载用了。 |
| /opt       | 这个是给第三方协力软体放置的目录 。 什么是第三方协力软体啊？举例来说，KDE这个桌面管理系统是一个独立的计画，不过他可以安装到Linux系统中，因此KDE的软体就建议放置到此目录下了。 另外，如果妳想要自行安装额外的软体(非原本的distribution提供的)，那么也能够将你的软体安装到这里来。 不过，以前的Linux系统中，我们还是习惯放置在/usr/local目录下。 |
| /root      | 系统管理员(root)的家目录。 之所以放在这里，是因为如果进入单人维护模式而仅挂载根目录时，该目录就能够拥有root的家目录，所以我们会希望root的家目录与根目录放置在同一个分区中。 |
| /usr/local | 管理员在本机自行安装自己下载的软件(非distribution默认提供者)，建议安装到此目录 |
| /var       | 安装时预留的动态空间,缓存文件和mysql等数据库文件、日志文件常常在此 |

==为何执行文件时常常会在前面加上`./`==

**原因:**由于指令的执行需要变量的支持，若你的执行文件放置在本目录，并且本目录并非正规的执行文件目录(/bin, /usr/bin等为正规)，此时要执行指令就得要严格指定该执行档。./代表本目录的意思，所以./run.sh代表执行本目录下， 名为run.sh的文件。

###### 常用和重要的配置文件

- /etc/inittab  系统运行级别的配置文件
- /etc/sudoers 用户权限配置文件
- /etc/ssh/sshd_config ssh连接配置文件
- /etc/init.d/ 所有服务的预设启动script都放在这里 
- /etc/passwd, /etc/shadow, /etc/group 用户的账号密码分组等信息

#### 修改软件源

1. 备份原始文件

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
```

2. 修改文件并添加国内源

```
vi /etc/apt/sources.list
```

3. 注释原文件内的源并添加如下地址

```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse

```

5. 更新源

```
sudo apt-get update
```

#### Linux用户管理

##### linux下添加用户并赋予root权限

1、添加用户，首先用adduser命令添加一个普通用户，命令如下：

`adduser tommy`

//添加一个名为tommy的用户

`passwd tommy`   //修改密码

Changing password for user tommy.

New UNIX password:     //在这里输入新密码

Retype new UNIX password:  //再次输入新密码

passwd: all authentication tokens updated successfully.

 

2、赋予root权限

方法一：修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉

\## Allows people in group wheel to run all commands
%wheel    ALL=(ALL)    ALL

然后修改用户，使其属于root组（wheel），命令如下：

`usermod -g root tommy`

修改完毕，现在可以用tommy帐号登录，然后用命令 su – ，即可获得root权限进行操作。

**方法二：**修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：

\## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
tommy   ALL=(ALL)     ALL

修改完毕，现在可以用tommy帐号登录，然后用命令 sudo – ，即可获得root权限进行操作。

##### 删除账号

命令:

`userdel`

选项:

  -r,  把用户的主目录一起删除。

例如:` userdel -r sam`

**修改帐号**
修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。
修改已有用户的信息使用usermod命令.
语法:
`usermod 选项 用户名`

选项:
包括-c, -d, -m, -g, -G, -s, -u以及-o等

例如:修改用户所在组

`usermod -g root tom`将tom的用户组改为root

**给已有的用户增加工作组**

`usermod -G groupname username`





#### 常用指令

- `pwd` 显示当前目录
- `ls` 
  - `ls -a` 显示隐藏文件
  - `ls -l` 一行一行的显示更详细的信息
- `cd`
  - `cd ~` 回到家目录
  - `cd ../`回到上一层
- mv 移动文件或者重命名
- `mkdir`创建目录 `mkdir /home/dir`
  - `mkdir -p /home/dir/pig`创建多级目录 
- `rmdir`删除**空目录**
  - `rm -rf 目录路径`删除非空目录
- `rm`  删除文件或者目录
  - `-f`不提示信息
  - `-r `递归删除文件夹下面的问价和目录
- `touch`创建一个空文件
  - touch text1 text2
- `cp source dest`将source拷贝到dest
  - `cp -r` 递归拷贝
- `cat`以只读方式查看文件
  - `-n`显示行号
- `more`
- `less` 根据需要加载内容,打开大型文件很快
  - `/`+要查找的字符串:向下寻找
  - `?`+要查找的字符串:向上寻找
  - `n`向上查找`N`向下查找
- `ln`
  - 创建软链接: `ln  -s`  [源文件或目录]  [目标文件或目录]
  -  创建硬链接: `ln`    [源文件或目录]  [目标文件或目录]

####export添加环境变量

#### 二、Linux 环境变量（export命令）

**环境变量启动过程：**

![img](https://images2015.cnblogs.com/blog/1021265/201707/1021265-20170723110839215-975002391.png)

**功能说明：**

　　设置或显示环境变量。（比如我们要用一个命令，但这个命令的执行文件不在当前目录，这样我们每次用的时候必须指定执行文件的目录，麻烦，在代码中先执行export，这个相当于告诉程序，执行某某东西时，需要的文件或什么东东在这些目录里）

**语　　法：**export [-fnp][变量名称]=[变量设置值]

**补充说明：**在shell中执行程序时，shell会提供一组环境变量。 export可新增，修改或删除环境变量，供后续执行的程序使用。export的效力仅及于该此登陆操作。

**参　　数：**

​    -f 　代表[变量名称]中为函数名称。 

　-n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。 

　-p 　列出所有的shell赋予程序的环境变量。

　　一个变量创建时，它不会自动地为在它之后创建的shell进程所知。而命令export可以向后面的shell传递变量的值。当一个shell脚本调用并执行时，它不会自动得到原为脚本（调用者）里定义的变量的访问权，除非这些变量已经被显式地设置为可用。export命令可以用于传递一个或多个变量的值到任何后继脚本。     ----《UNIX教程》

　　 一般来说，配置交叉编译工具链的时候需要指定编译工具的路径，此时就需要设置环境变量。例如我的mips-linux-gcc编译器在“/opt/au1200_rm /build_tools/bin”目录下，build_tools就是我的编译工具，则有如下三种方法来设置环境变量：

**1、直接用export命令：**

```
#export PATH=$PATH:/opt/au1200_rm/build_tools/bin
```

查看是否已经设好，可用命令export查看：

**2、修改profile文件：**

```
#vi /etc/profile
在里面加入:
export PATH="$PATH:/opt/au1200_rm/build_tools/bin"
```

**3. 修改.bashrc文件：**

```
# vi /root/.bashrc
在里面加入：
export PATH="$PATH:/opt/au1200_rm/build_tools/bin"
```

后两种方法一般需要重新注销系统才能生效，最后可以通过echo命令测试一下：
\# echo $PATH

![img](https://images2015.cnblogs.com/blog/1021265/201707/1021265-20170723110346215-1051406925.png)
看看输出里面是不是已经有了 /my_new_path这个路径了。

　　“/bin”、“/sbin”、“/usr/bin”、“/usr/sbin”、“/usr/local/bin”等路径已经在系统环境变量中了，如果可执行文件在这几个标准位置，在终端命令行输入该软件可执行文件的文件名和参数(如果需要参数)，回车即可。 　　

　　如果不在标准位置，文件名前面需要加上完整的路径。不过每次都这样跑就太麻烦了，一个“一劳永逸”的办法是把这个路径加入环境变量。命令 “PATH=$PATH:路径”可以把这个路径加入环境变量，但是退出这个命令行就失效了。要想永久生效，需要把这行添加到环境变量文件里。有两个文件可 选：“/etc/profile”和用户主目录下的“.bash_profile”，“/etc/profile”对系统里所有用户都有效，用户主目录下 的“.bash_profile”只对这个用户有效。 　　

　　“PATH=PATH:路径1:路径2:...:路径n”，意思是可执行文件的路径包括原先设定的路径，也包括从“路径1”到“路径n”的所有路径。当用户输入一个一串字符并按回车后，shell会依次在这些路径里找对应的可执行文件并交给系统核心执行。那个“PATH:路径1:路径2:...:路径n”，意思是可执行文件的路径包括原先设定的路径，也包括从“路径1”到“路径n”的所有路径。当用户输入一个一串字符并按回车后，shell会依次在这些路径里找对应的可执行文件并交给系统核心执行。那个“PATH”表示原先设定的路径 仍然有效，注意不要漏掉。某些软件可能还有“PATH”以外类型的环境变量需要添加，但方法与此相同，并且也需要注意“$”。 　　

注意，与DOS/Window不同，UNIX类系统环境变量中路径名用冒号分隔，不是分号。另外，软件越装越多，环境变量越添越多，为了避免造成混乱，建议所有语句都添加在文件结尾，按软件的安装顺序添加。 　　

格式如下()： 　　

\# 软件名-版本号 　　

PATH=$PATH:路径1:路径2:...:路径n 　　

其他环境变量=$其他环境变量:... 　　

在“profile”和“.bash_profile”中，“#”是注释符号，写在这里除了视觉分隔外没有任何效果。 　　

设置完毕，注销并重新登录，设置就生效了。如果不注销，直接在shell里执行这些语句，也能生效，但是作用范围只限于执行了这些语句的shell。 　　

相关的环境变量生效后，就不必老跑到软件的可执行文件目录里去操作了。

 

![img](https://images2015.cnblogs.com/blog/1021265/201707/1021265-20170723104232871-1110656253.png)

![img](https://images2015.cnblogs.com/blog/1021265/201707/1021265-20170723104242559-1809151278.png)

![img](https://images2015.cnblogs.com/blog/1021265/201707/1021265-20170723105427668-1432143954.png)

#### Linux执行任务常用操作(jobs, fg, bg, &)

###### 一。& 最经常被用到

这个用在一个命令的最后，可以把这个命令放到后台执行

###### 二。ctrl + z

可以将一个正在前台执行的命令放到后台，并且暂停

###### 三。jobs

查看当前有多少在后台运行的命令

###### 四。fg

将后台中的命令调至前台继续运行
如果后台中有多个命令，可以用 `fg %jobnumber`将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)

###### 五。bg

将一个在后台暂停的命令，变成继续执行
如果后台中有多个命令，可以用bg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)

######  六 ctrl+c  

用于前台进程的终止

#### 后台运行程序的nohub命令

###### 介绍:

`nohup` 是 **no hang up** 的缩写，就是不挂断的意思。

`nohup`命令：如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用`nohup`命令。该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。

在缺省情况下该作业的所有输出都被重定向到一个名为`nohup.out`的文件中。

###### 使用:

`&` ： 指在后台运行

`nohup` ： 不挂断的运行，注意并没有后台运行的功能，就是指，用`nohup`运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响他的运行，注意了`nohup`没有后台运行的意思；

`&`才是是指在后台运行，但当用户推出(挂起)的时候，命令自动也跟着退出

我们可以巧妙的吧他们结合起来用就是`nohup COMMAND &`, 这样就能使命令永久的在后台执行

###### 举例:

1. `sh test.sh &  `

   将sh test.sh任务放到后台 ，关闭xshell，对应的任务也跟着停止。

2. `nohup sh test.sh `
   将sh test.sh任务放到后台，关闭标准输入，**终端不再能够接收任何输入（标准输入）**，重定向标准输出和标准错误到当前目录下的nohup.out文件，即使关闭xshell退出当前session依然继续运行。

3. `nohup sh test.sh  & `
   将sh test.sh任务放到后台，但是依然可以使用标准输入，**终端能够接收任何输入**，重定向标准输出和标准错误到当前目录下的nohup.out文件，即使关闭xshell退出当前session依然继续运行。

### Linux服务器查看应用内存占用情况

查看RAM使用情况最简单的方法是通过/proc/meminfo。这个动态更新的虚拟文件实际上是许多其他内存相关工具(如：free / ps / top)等的组合显示。/proc/meminfo列出了所有你想了解的内存的使用情况。

```bash
$ cat /proc/meminfo
```

ps命令可以实时的显示各个进程的内存使用情况。Reported memory usage information includes %MEM (percent of physical memory used), VSZ (total amount of virtual memory used), and RSS (total amount of physical memory used)。你可以使用 “–sort”选项对进程进行排序，例如按RSS进行排序：

```bash
 ps aux --sort -rss
```

![1572880234356](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1572880234356.png)

#### Linux用vim编辑文件未正常退出的解决办法

Linux下编程难免会开启多次vim编辑， 同一个文件如果在上一次编辑时未进行保存，则在下一次想要进行编辑时就会出现：

swap file "*.swp" already exists!

[O]pen Read-Only, (E)dit anyway, (R)ecover, (D)elete it, (Q)uit, (A)bort:

原因：

*使用vim编辑文件实际是先copy一份临时文件并映射到内存给你编辑， 编辑的是临时文件， 当执行：w后才保存临时文件到原文件，执行：q后才删除临时文件。*

*每次启动编辑时都会检索这个文件是否已经存在临时文件， 有则询问如何处理，就会出现如上情景。*

 解决方法（删除这个临时文件即可）：

rm *.swp