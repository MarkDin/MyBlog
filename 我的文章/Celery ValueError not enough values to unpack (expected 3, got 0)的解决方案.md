# Celery ValueError: not enough values to unpack (expected 3, got 0)的解决方案

## 场景还原

本地环境如下：

- Windows 10
- Python 3.5.2
- Celery 4.1.0

代码tasks.py：

from celery import Celery

app = Celery('tasks', broker='redis://:xxxx@xxx.xxx.xxx.xx:6379/0')

@app.task
def add(x, y):
    return x + y
1
2
3
4
5
6
7
执行worker

celery -A tasks worker --loglevel=info
1
输出：

 -------------- celery@kong9wei11 v4.1.0 (latentcall)
---- **** -----
--- * ***  * -- Windows-10-10.0.14393-SP0 2018-01-12 19:01:39
-- * - **** ---
- ** ---------- [config]
- ** ---------- .> app:         tasks:0x157fd248550
- ** ---------- .> transport:   redis://:**@114.67.225.0:6379/0
- ** ---------- .> results:     disabled://
- *** --- * --- .> concurrency: 4 (prefork)
-- ******* ---- .> task events: OFF (enable -E to monitor tasks in this worker)
--- ***** -----
 -------------- [queues]
                .> celery           exchange=celery(direct) key=celery


[tasks]
  . tasks.add

[2018-01-12 19:01:40,029: INFO/MainProcess] Connected to redis://:**@114.67.225.0:6379/0
[2018-01-12 19:01:40,130: INFO/MainProcess] mingle: searching for neighbors
[2018-01-12 19:01:40,550: INFO/SpawnPoolWorker-1] child process 9048 calling self.run()
[2018-01-12 19:01:40,557: INFO/SpawnPoolWorker-2] child process 9028 calling self.run()
[2018-01-12 19:01:40,578: INFO/SpawnPoolWorker-3] child process 13064 calling self.run()
[2018-01-12 19:01:40,611: INFO/SpawnPoolWorker-4] child process 9856 calling self.run()
[2018-01-12 19:01:41,693: INFO/MainProcess] mingle: all alone
[2018-01-12 19:01:42,212: INFO/MainProcess] celery@xx ready.
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
调用任务代码manager.py：

```python
from tasks import add
add.delay(4, 4)
```

执行

```python
python manager.py
1
```


然后worker里报错：

```
[2018-01-12 19:08:15,545: INFO/MainProcess] Received task: tasks.add[5d387722-5389-441b-9b01-a619b93b4702]
[2018-01-12 19:08:15,550: ERROR/MainProcess] Task handler raised error: ValueError('not enough values to unpack (expected 3, got 0)',)
Traceback (most recent call last):
  File "d:\programmingsoftware\python35\lib\site-packages\billiard\pool.py", line 358, in workloop
    result = (True, prepare_result(fun(*args, **kwargs)))
  File "d:\programmingsoftware\python35\lib\site-packages\celery\app\trace.py", line 525, in _fast_trace_task
    tasks, accept, hostname = _loc
ValueError: not enough values to unpack (expected 3, got 0)
```

## 解决：

看别人描述大概就是说win10上运行celery4.x就会出现这个问题，解决办法如下,原理未知：

先安装一个`eventlet

pip install eventlet
1
然后启动worker的时候加一个参数，如下：

celery -A <mymodule> worker -l info -P eventlet
1
然后就可以正常的调用了。

