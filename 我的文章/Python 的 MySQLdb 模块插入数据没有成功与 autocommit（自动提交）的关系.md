常数操作:操作和计算次数和样本量大小无关

# Python 的 MySQLdb 模块插入数据没有成功与 autocommit（自动提交）的关系

2013年11月17日 17:30:09  更多



在使用PYTHON mysqldb的时候插入数据发现 数据库没有你当前插入的数据，这时候实际上跟commit有关系



用 MySQLdb 操作数据库，插入数据之后发现数据库中依然为空，不知原因为何。 

开启 mysqld 的 log 设置项之后发现日志文档中更有执行 sql 语句，直接复制语句在客户端中执行也没有问题，那么为什么通过 MySQLdb 的插入全部没有结果呢？ 

我怀疑是 MySQLdb 的问题，在日志文件中仔细的看了一遍运行的所有sql 语句，在建立连接之后还运行了这句：set autocommit=0。这句话的嫌疑很大，因为这个涉及到一个语句提交执行的问题，而且对于 commit 我有点印象，好像以前学习 MySQLdb 的时候，特意注意到了这点。不管怎样，这就找准了关键字：MySQLdb autocommit 

根据网上搜到的结果，可以大概了解到，MySQLdb 在连接后关闭了自动提交，自动提交对于 innodb 引擎很重要，没有这个设置，innodb 引擎就不会真正执行语句。 

解决的办法： 
1、语句末尾加上“COMMIT;” 
2、运行完语句，至少在关闭数据库之前提交一下，如：conn.commit() 
3、数据库连接建立之后，设置自动提交，如：conn.autocommit(1) 

只是不知道为什么 innodb 会这样，可能是因为这是一个事务型数据库引擎，没有提交就不会在服务器上执行，只会缓存在客户端上的缘故吧！ 



# python-sqlalchemy中设置autocommit

SQLALCHEMY_DATABASE_URI = ('mysql+pymysql://username:passwrod@ip:prot/database?charset=utf8&autocommit=true')



错误是sqlalchemy抛出。原因是你从pool拿的connection没有以session.commit或者session.rollback或者session.close的某一种放回pool里。这时connection的transaction没有完结（rollback or commit)。
而不知什么原因（recyle了，timeout了）你的connection又死掉了，你的sqlalchemy尝试重新连接。由于transaction还没完结，无法重连。
正确用法是确保每个web请求完了session.close或者session.rollback。把链接还回pool。

## 经典快排的缺陷

每次固定选择一个数作为划分的对象, 这样与数据状况有关, 有时候效率比较低

