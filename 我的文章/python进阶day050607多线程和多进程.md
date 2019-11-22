# 多线程和多进程

## python中的GIL

全称是`global interpreter lock`, 它使得同一时刻只有一个线程在cpu上面运行字节码

**也使得无法将多个线程映射到多个CPU上面**

GIL会根据执行的字节码的行数以及CPU时间片来释放, 并不会一直拥有直到进程结束

GIL会在遇到IO操作的时候会释放, 所以多线程在IO密集的时候效率高

### 多线程

对于IO操作来说, 多进程和多线程差别不大

#### 线程间通信

##### 消息队列Queue(线程安全)

###### 方法:

qsize(): 获取长度

empty()

full()

put()

get()

put_nowait()

get_nowait()

**join()**从队列的角度阻塞主线程, 函数想要退出, 必须要发送一个task_down信号

**task_down()**

#### 线程同步

#####Lock

lock = threading.Lock()

lock.acquire()

lock.release()

###### 锁会影响性能

锁会带来死锁 :比如连续lock.acquire()两次就会造成死锁, 当我们调用子函数且子函数中需要acquire的时候, 就非常容易造成死锁

##### RLock, 可重入的锁

注意:

**同一线程**中acquire的次数和release的次数一定要相等

##### condition 条件变量

用于复杂的线程间同步

###### 用法:

在需要同步的代码段使用`with condition:`

然后在with中通过notify()和wait()方法来实现唤醒和等待

注意: notify和wait的顺序很重要, 线程启动的顺序也很重要

```python
#!/usr/bin/python 
# -*- coding: utf-8 -*-
# @Author: 丁珂
# @Time: 2019/11/11 15:40
import threading
from threading import Condition


class XiaoAi(threading.Thread):
    def __init__(self, condition: Condition):
        super(XiaoAi, self).__init__(name="小爱")
        self.condition = condition

    def run(self):
        with self.condition:
            self.condition.wait()
            print("{} : 在".format(self.name))
            self.condition.notify()


class TianMao(threading.Thread):
    def __init__(self, condition: Condition):
        super(TianMao, self).__init__(name="天猫")
        self.condition = condition

    def run(self):
        with self.condition:
            print("{} : 小爱同学".format(self.name))
            self.condition.notify()
            self.condition.wait()


if __name__ == '__main__':
    con = Condition()
    tianmao = TianMao(con)
    xiaoai = XiaoAi(con)

    xiaoai.start()
    tianmao.start()

```

#####Semaphore使用和源码解析

作用: 控制线程并发数量

### ThreadPoolExecutor线程池

##### 为什么要使用线程池

线程池可以获取某个线程或者任务的状态和返回值

当一个线程完成时我们主线程能够立马知道

`futures`包可以让多线程和多进程编码接口一致















