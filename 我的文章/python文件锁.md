版权声明：本文为博主原创文章，遵循 CC 4.0 by-sa 版权协议，转载请附上原文出处链接和本声明。
本文链接：https://blog.csdn.net/lianxiang_biancheng/article/details/7907776
1.文件锁

问题：进程P1中有一个线程T1，T1执行的函数为f1；进程P2中有一个线程T2，T2执行的函数为f2。

当f1和f2都需要对同一个资源进行操作时，比如同时对文件file1进行操作。为了线程安全，则当f1在操作（读或写文件file1）时，不允许f2操作（读或写文件file1）。反之，当f2在操作file1时，不允许f1操作file1。即f1和f2不能同时操作file1。

解决方法：

可以采用文件锁(这里文件锁的意思为将对资源file1的访问状态保存在文件fs.txt里，即通过文件fs.txt来加锁)的方式，对文件file1轮流交替的操作：即f1操作完file1之后，f2开始操作file1；当f2操作完file1之后，f1开始操作file1，这样交替下去。

可以设置4种状态：00、11、22、33。将这4种状态保存在文件‘fs.txt’里，因为这样进程P1和P2都可以操作文件fs.txt（解决了进程间相互通信的问题）。4种状态分别表示如下：

00：表示f1可以操作资源file1了，同时也表示f2操作完毕file1；

11：表示f1正在操作资源file1；

22：表示f1操作完毕file1，同时也表示f2可以操作file1了；

33：表示f2正在操作file1。

访问流程图如下所示：



我们可以看到，函数f1的状态顺序为'00' ->'11' -> '22'；函数f2的状态顺序为'22' -> '33' -> '00'。

形成了如下的环形交替访问：



2.Python中多进程的使用

下面将python中多进程的使用和文件锁进行结合，给出一个简单的demo。

公共函数（读文件、写文件）定义在文件GlobalFunc.py中：

# encoding: utf-8

#根据文件名，读取文件内容
def read_file(filename):
    all_the_text = ''
    fo = open(filename)
    try:
        all_the_text = fo.read()
    finally:
        fo.close()
    return all_the_text
    

#根据文件名和内容，写入文件。成功返回1
def write_file(filename, filecontent):
    status = 0
    try:
        fo = open(filename, 'wb+')
        fo.write(filecontent)
        status = 1
    finally:
        fo.close()
    return status

进程P2的文件Process2.py定义如下：
# encoding: utf-8
from threading import Thread
from time import sleep
from GlobalFunc import read_file,write_file

#定义线程T2的执行函数
def f2():
    fs = read_file('fs.txt')
    print '\nP2 want to visit,now fs:',fs
    if '22' == fs:  #f1操作完file1，f2可以开始操作了
        write_file('fs.txt','33') #表明f2正在操作file1

        print 'P2 is visiting file1...'
     
        write_file('fs.txt','00')
        sleep(10)
    else:
        sleep(1)

#定义线程T2
def T2():
    while True:
        f2()

def main():
    print '\nlauch process:P2...'
    #启动线程T2
    Thread(target = T2,args=()).start()
    while True:
        sleep(3)
    
if __name__ == '__main__':
    main()
进程P1的文件Process.py定义如下：

# encoding: utf-8
from threading import Thread
from time import sleep
from multiprocessing import Process
from GlobalFunc import read_file,write_file

#线程T1的执行函数
def f1():
    fs = read_file('fs.txt')
    print '\nP1 want to visit,now fs:',fs
    assert('00' == fs)
    if '00' == fs:  #f2操作完file1，f1可以开始操作了
        write_file('fs.txt','11') #表明f1正在操作file1

        print 'P1 is visiting file1...'
     
        write_file('fs.txt','22')
        sleep(10)
    else:
        sleep(1)

#线程T1
def T1():
    while True:
        f1()

if __name__ == '__main__':
    print 'lauch process:P1...'
    #初始化'fs.txt'
    write_file('fs.txt','00')

    #进程P2的定义
    from Process2 import main as P2_main
    P2 = Process(target = P2_main, args=())
    P2.start()
    
    #启动线程T1
    Thread(target = T1,args=()).start()
    
    while True:
        sleep(3)
进程P2单独定义成一个文件'Process2.py'。通过进程P1来调用启动进程P2，进程P1中有一个线程T1，进程P2中有一个线程T2，T1和T2都对文件file1进行操作。我们通过'fs.txt'这个文件设置文件锁，将T1和T2的当前操作状态保存在fs.txt中。



注：我们通过文件锁，解决了进程之间互斥操作同一个资源的问题。如果是同一个线程之间互斥操作同一个资源的问题，我们只需要定义个全局变量即可，我们没有必要使用文件锁，因为文件锁需要访问磁盘。
 ———————————————— 
版权声明：本文为CSDN博主「lianxiang_biancheng」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lianxiang_biancheng/article/details/7907776