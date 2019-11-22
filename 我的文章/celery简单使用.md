版权声明：本文为博主原创文章，遵循 CC 4.0 by-sa 版权协议，转载请附上原文出处链接和本声明。
本文链接：https://blog.csdn.net/tichimi3375/article/details/82415412
目录

celery介绍

Task Queue

Celery安装 

Broker

Application

1.创建应用

2.调用任务

3.存储结果

配置

1.直接通过app来配置

2.专有配置文件

项目中使用Celery

调用任务（Calling Task）

Designing Work-flows

Routing

Periodic Tasks

Django使用Celery

1. 配置celery

2. 存储任务结果

3. 定时任务

1. 问题抛出
我们在做网站后端程序开发时，会碰到这样的需求：用户需要在我们的网站填写注册信息，我们发给用户一封注册激活邮件到用户邮箱，如果由于各种原因，这封邮件发送所需时间较长，那么客户端将会等待很久，造成不好的用户体验.



那么怎么解决这样的问题呢?



我们将耗时任务放到后台异步执行。不会影响用户其他操作。除了注册功能，例如上传，图形处理等等耗时的任务，都可以按照这种思路来解决。 如何实现异步执行任务呢？我们可使用celery. celery除了刚才所涉及到的异步执行任务之外，还可以实现定时处理某些任务。

celery介绍
Celery是一个功能完备即插即用的任务队列。它使得我们不需要考虑复杂的问题，使用非常简单。celery看起来似乎很庞大，本章节我们先对其进行简单的了解，然后再去学习其他一些高级特性。 celery适用异步处理问题，当发送邮件、或者文件上传, 图像处理等等一些比较耗时的操作，我们可将其异步执行，这样用户不需要等待很久，提高用户体验。 celery的特点是：

简单，易于使用和维护，有丰富的文档。
高效，单个celery进程每分钟可以处理数百万个任务。
灵活，celery中几乎每个部分都可以自定义扩展。
celery非常易于集成到一些web开发框架中.

Task Queue
任务队列是一种跨线程、跨机器工作的一种机制.

  任务队列中包含称作任务的工作单元。有专门的工作进程持续不断的监视任务队列，并从中获得新的任务并处理.

  celery通过消息进行通信，通常使用一个叫Broker(中间人)来协client(任务的发出者)和worker(任务的处理者). clients发出消息到队列中，broker将队列中的信息派发给worker来处理。

  一个celery系统可以包含很多的worker和broker，可增强横向扩展性和高可用性能。



Celery安装 
我们可以使用python的包管理器pip来安装:

pip install -U Celery
也可从官方直接下载安装包:https://pypi.python.org/pypi/celery/

tar xvfz celery-0.0.0.tar.gz
cd celery-0.0.0
python setup.py build
python setup.py install
 Broker
Celery需要一种解决消息的发送和接受的方式，我们把这种用来存储消息的的中间装置叫做message broker, 也可叫做消息中间人。 作为中间人，我们有几种方案可选择：

1.RabbitMQ
RabbitMQ是一个功能完备，稳定的并且易于安装的broker. 它是生产环境中最优的选择。使用RabbitMQ的细节参照以下链接： http://docs.celeryproject.org/en/latest/getting-started/brokers/rabbitmq.html#broker-rabbitmq

如果我们使用的是Ubuntu或者Debian发行版的Linux，可以直接通过下面的命令安装RabbitMQ: sudo apt-get install rabbitmq-server 安装完毕之后，RabbitMQ-server服务器就已经在后台运行。如果您用的并不是Ubuntu或Debian, 可以在以下网址： http://www.rabbitmq.com/download.html 去查找自己所需要的版本软件。

2.Redis
Redis也是一款功能完备的broker可选项，但是其更可能因意外中断或者电源故障导致数据丢失的情况。 关于是有那个Redis作为Broker，可访下面网址： http://docs.celeryproject.org/en/latest/getting-started/brokers/redis.html#broker-redis

Application
  使用celery第一件要做的最为重要的事情是需要先创建一个Celery实例，我们一般叫做celery应用，或者更简单直接叫做一个app。app应用是我们使用celery所有功能的入口，比如创建任务，管理任务等，在使用celery的时候，app必须能够被其他的模块导入。

1.创建应用
我们首先创建tasks.py模块, 其内容为:

from celery import Celery

# 我们这里案例使用redis作为broker
app = Celery('demo', broker='redis://:332572@127.0.0.1/1')

# 创建任务函数
@app.task
def my_task():
    print("任务函数正在执行....")
  Celery第一个参数是给其设定一个名字， 第二参数我们设定一个中间人broker, 在这里我们使用Redis作为中间人。my_task函数是我们编写的一个任务函数， 通过加上装饰器app.task, 将其注册到broker的队列中。

  现在我们在创建一个worker， 等待处理队列中的任务.打开终端，cd到tasks.py同级目录中，执行命令:

celery -A tasks worker --loglevel=info
显示效果如下:

 

2.调用任务
  任务加入到broker队列中，以便刚才我们创建的celery workder服务器能够从队列中取出任务并执行。如何将任务函数加入到队列中，可使用delay()。

进入python终端, 执行如下代码:

from tasks import my_task
my_task.delay()
执行效果如下: 

  

我们通过worker的控制台，可以看到我们的任务被worker处理。调用一个任务函数，将会返回一个AsyncResult对象，这个对象可以用来检查任务的状态或者获得任务的返回值。

3.存储结果
  如果我们想跟踪任务的状态，Celery需要将结果保存到某个地方。有几种保存的方案可选:SQLAlchemy、Django ORM、Memcached、 Redis、RPC (RabbitMQ/AMQP)。

  例子我们仍然使用Redis作为存储结果的方案，任务结果存储配置我们通过Celery的backend参数来设定。我们将tasks模块修改如下:

from celery import Celery

# 我们这里案例使用redis作为broker
app = Celery('demo',
             backend='redis://:332572@127.0.0.1:6379/2',
             broker='redis://:332572@127.0.0.1:6379/1')

# 创建任务函数
@app.task
def my_task(a, b):
    print("任务函数正在执行....")
    return a + b
  我们给Celery增加了backend参数，指定redis作为结果存储,并将任务函数修改为两个参数，并且有返回值。 

更多关于result对象信息，请参阅下列网址:http://docs.celeryproject.org/en/latest/reference/celery.result.html#module-celery.result

配置
  Celery使用简单，配置也非常简单。Celery有很多配置选项能够使得celery能够符合我们的需要，但是默认的几项配置已经足够应付大多数应用场景了。

  配置信息可以直接在app中设置，或者通过专有的配置模块来配置。

1.直接通过app来配置
from celery import Celery
app = Celery('demo')
# 增加配置
app.conf.update(
    result_backend='redis://:332572@127.0.0.1:6379/2',
    broker_url='redis://:332572@127.0.0.1:6379/1',
)
2.专有配置文件
  对于比较大的项目，我们建议配置信息作为一个单独的模块。我们可以通过调用app的函数来告诉Celery使用我们的配置模块。

  配置模块的名字我们取名为celeryconfig, 这个名字不是固定的，我们可以任意取名，建议这么做。我们必须保证配置模块能够被导入。 配置模块的名字我们取名为celeryconfig, 这个名字不是固定的，我们可以任意取名，建议这么做。我们必须保证配置模块能够被导入。

  下面我们在tasks.py模块 同级目录下创建配置模块celeryconfig.py:

result_backend = 'redis://:332572@127.0.0.1:6379/2'
broker_url = 'redis://:332572@127.0.0.1:6379/1'
  tasks.py文件修改为:

from celery import Celery
import celeryconfig

# 我们这里案例使用redis作为broker
app = Celery('demo')

# 从单独的配置模块中加载配置
app.config_from_object('celeryconfig')
更多配置: http://docs.celeryproject.org/en/latest/userguide/configuration.html#configuration

项目中使用Celery
  我的项目目录:

TestCelery/ ├── proj │ ├── celeryconfig.py │ ├── celery.py │ ├── init.py │ └── tasks.py └── test.py

  celery.py内容如下:

from celery import Celery

# 创建celery实例
app = Celery('demo')
app.config_from_object('proj.celeryconfig')

# 自动搜索任务
app.autodiscover_tasks(['proj'])
  celeryconfig.p模块内容如下：

from kombu import Exchange, Queue
BROKER_URL = 'redis://:332572@127.0.0.1:6379/1'
CELERY_RESULT_BACKEND = 'redis://:332572@127.0.0.1:6379/2'
  tasks.py模块内容如下:

from proj.celery import app as celery_app

# 创建任务函数
@celery_app.task
def my_task1():
    print("任务函数(my_task1)正在执行....")

@celery_app.task
def my_task2():
    print("任务函数(my_task2)正在执行....")

@celery_app.task
def my_task3():
    print("任务函数(my_task3)正在执行....")
  启动worker:

celery -A proj worker -l info


键入ctrl+c可关闭worker.

调用任务（Calling Task）
调用任务，可使用delay()方法:

my_task.delay(2, 2)
  也可以使用apply_async()方法，该方法可让我们设置一些任务执行的参数，例如，任务多久之后才执行，任务被发送到那个队列中等等.

my_task.apply_async((2, 2), queue='my_queue', countdown=10)
任务my_task将会被发送到my_queue队列中，并且在发送10秒之后执行。

  如果我们直接执行任务函数，将会直接执行此函数在当前进程中，并不会向broker发送任何消息。

  无论是delay()还是apply_async()方式都会返回AsyncResult对象，方便跟踪任务执行状态，但需要我们配置result_backend.

每一个被吊用的任务都会被分配一个ID，我们叫Task ID.

