# [SQLAlchemy基本使用](https://www.cnblogs.com/gaoya666/p/9205021.html)

### 介绍

SQLAlchemy是一个基于Python实现的ORM框架。该框架建立在 DB API之上，使用关系对象映射进行数据库操作，简言之便是：将类和对象转换成SQL，然后使用数据API执行SQL并获取执行结果。

#### 作用

帮助我们使用类和对象快速实现数据库操作。

#### 安装

```
`pip3 install sqlalchemy`
```

![img](https://images2018.cnblogs.com/blog/1299726/201806/1299726-20180620173858806-1347766099.png)

#### 组成部分：

- Engine，框架的引擎
- Connection Pooling ，数据库连接池
- Dialect，选择连接数据库的DB API种类
- Schema/Types，架构和类型
- SQL Exprression Language，SQL表达式语言

####  示例

\- 用户登录示例
\- 用户注册示例
\- 数据库获取数据实时更新（重写构造方法）

### 使用

#### 创建数据库单表

```python
`#!/usr/bin/env python``# -*- coding:utf-8 -*-``import` `datetime``from` `sqlalchemy ``import` `create_engine``from` `sqlalchemy.ext.declarative ``import` `declarative_base``from` `sqlalchemy ``import` `Column, Integer, String, Text, ForeignKey, DateTime, UniqueConstraint, Index` `Base ``=` `declarative_base()`  `class` `Users(Base):``    ``__tablename__ ``=` `'users'` `    ``id` `=` `Column(Integer, primary_key``=``True``)``    ``name ``=` `Column(String(``32``), index``=``True``, nullable``=``False``)``    ``# email = Column(String(32), unique=True)``    ``# ctime = Column(DateTime, default=datetime.datetime.now)``    ``# extra = Column(Text, nullable=True)` `    ``__table_args__ ``=` `(``        ``# UniqueConstraint('id', 'name', name='uix_id_name'),``        ``# Index('ix_id_name', 'name', 'email'),``    ``)`  `def` `init_db():``    ``"""``    ``根据类创建数据库表``    ``:return:``    ``"""``    ``engine ``=` `create_engine(``        ``"mysql+pymysql://root:123@127.0.0.1:3306/s6?charset=utf8"``,    ``#用户名：密码@本机ip:数据库端口3306/数据库名？字符集``        ``max_overflow``=``0``,  ``# 超过连接池大小外最多创建的连接``        ``pool_size``=``5``,  ``# 连接池大小``        ``pool_timeout``=``30``,  ``# 池中没有线程最多等待的时间，否则报错``        ``pool_recycle``=``-``1`  `# 多久之后对线程池中的线程进行一次连接的回收（重置）``    ``)` `    ``Base.metadata.create_all(engine)`  `def` `drop_db():``    ``"""``    ``根据类删除数据库表``    ``:return:``    ``"""``    ``engine ``=` `create_engine(``        ``"mysql+pymysql://root:123@127.0.0.1:3306/s6?charset=utf8"``,``        ``max_overflow``=``0``,  ``# 超过连接池大小外最多创建的连接``        ``pool_size``=``5``,  ``# 连接池大小``        ``pool_timeout``=``30``,  ``# 池中没有线程最多等待的时间，否则报错``        ``pool_recycle``=``-``1`  `# 多久之后对线程池中的线程进行一次连接的回收（重置）``    ``)` `    ``Base.metadata.drop_all(engine)`  `if` `__name__ ``=``=` `'__main__'``:``    ``drop_db()``    ``init_db() `
```

#### 操作数据库表

```python
`#!/usr/bin/env python``# -*- coding:utf-8 -*-``from` `sqlalchemy.orm ``import` `sessionmaker``from` `sqlalchemy ``import` `create_engine``from` `models ``import` `Users``  ` `engine ``=` `create_engine(``"mysql+pymysql://root:123456@127.0.0.1:3306/s6"``, max_overflow``=``0``, pool_size``=``5``)``Session ``=` `sessionmaker(bind``=``engine)``  ` `# 每次执行数据库操作时，都需要创建一个session``session ``=` `Session()``  ` `# ############# 执行ORM操作 #############``obj1 ``=` `Users(name``=``"alex1"``)``session.add(obj1)``  ` `# 提交事务``session.commit()``# 关闭session``session.close()　　 `
```

#### 基本增删改查

```python
`#!/usr/bin/env python``# -*- coding:utf-8 -*-``import` `time``import` `threading` `from` `sqlalchemy.ext.declarative ``import` `declarative_base``from` `sqlalchemy ``import` `Column, Integer, String, ForeignKey, UniqueConstraint, Index``from` `sqlalchemy.orm ``import` `sessionmaker, relationship``from` `sqlalchemy ``import` `create_engine``from` `sqlalchemy.sql ``import` `text` `from` `db ``import` `Users, Hosts` `engine ``=` `create_engine(``"mysql+pymysql://root:123456@127.0.0.1:3306/s6"``, max_overflow``=``0``, pool_size``=``5``)``Session ``=` `sessionmaker(bind``=``engine)` `session ``=` `Session()` `# ################ 添加 ################``"""``obj1 = Users(name="yaya")``session.add(obj1)` `session.add_all([``    ``Users(name="yaya"),``    ``Users(name="xiaoqiang"),``    ``Hosts(name="c1.com"),``])``session.commit()``"""` `# ################ 删除 ################``"""``session.query(Users).filter(Users.id > 2).delete()``session.commit()``"""``# ################ 修改 ################``"""``session.query(Users).filter(Users.id > 0).update({"name" : "099"})``session.query(Users).filter(Users.id > 0).update({Users.name: Users.name + "099"}, synchronize_session=False)``session.query(Users).filter(Users.id > 0).update({"age": Users.age + 1}, synchronize_session="evaluate")``session.commit()``"""``# ################ 查询 ################``"""``r1 = session.query(Users).all()``r2 = session.query(Users.name.label('xx'), Users.age).all()``r3 = session.query(Users).filter(Users.name == "yaya").all()``r4 = session.query(Users).filter_by(name='alex').all()``r5 = session.query(Users).filter_by(name='alex').first()``r6 = session.query(Users).filter(text("id<:value and name=:name")).params(value=224, name='fred').order_by(Users.id).all()``r7 = session.query(Users).from_statement(text("SELECT * FROM users where name=:name")).params(name='ed').all()``"""` `session.close()`
```

#### 其他常用操作　　

```python
`# ############################## 其他常用 ###############################``# 1. 指定列`` ``select ``id``,name as cname ``from` `users;`` ``result ``=` `session.query(Users.``id``,Users.name.label(``'cname'``)).``all``()`` ``for` `item ``in` `result:``         ``print``(item[``0``],item.``id``,item.cname)``# 2. 默认条件and`` ``session.query(Users).``filter``(Users.``id` `> ``1``, Users.name ``=``=` `'eric'``).``all``()``# 3. between`` ``session.query(Users).``filter``(Users.``id``.between(``1``, ``3``), Users.name ``=``=` `'eric'``).``all``()``# 4. in`` ``session.query(Users).``filter``(Users.``id``.in_([``1``,``3``,``4``])).``all``()`` ``session.query(Users).``filter``(~Users.``id``.in_([``1``,``3``,``4``])).``all``()``# 5. 子查询`` ``session.query(Users).``filter``(Users.``id``.in_(session.query(Users.``id``).``filter``(Users.name``=``=``'eric'``))).``all``()``# 6. and 和 or`` ``from` `sqlalchemy ``import` `and_, or_`` ``session.query(Users).``filter``(Users.``id` `> ``3``, Users.name ``=``=` `'eric'``).``all``()`` ``session.query(Users).``filter``(and_(Users.``id` `> ``3``, Users.name ``=``=` `'eric'``)).``all``()`` ``session.query(Users).``filter``(or_(Users.``id` `< ``2``, Users.name ``=``=` `'eric'``)).``all``()`` ``session.query(Users).``filter``(``     ``or_(``         ``Users.``id` `< ``2``,``         ``and_(Users.name ``=``=` `'eric'``, Users.``id` `> ``3``),``         ``Users.extra !``=` `""``     ``)).``all``()` `# 7. filter_by`` ``session.query(Users).filter_by(name``=``'alex'``).``all``()` `# 8. 通配符`` ``ret ``=` `session.query(Users).``filter``(Users.name.like(``'e%'``)).``all``()    ``#e% 以e开头的所有，e_ 以e开头的一个`` ``ret ``=` `session.query(Users).``filter``(~Users.name.like(``'e%'``)).``all``()   ``#~ 指 not in` `# 9. 切片(限制)`` ``result ``=` `session.query(Users)[``1``:``2``]` `# 10.排序`` ``ret ``=` `session.query(Users).order_by(Users.name.desc()).``all``()`` ``ret ``=` `session.query(Users).order_by(Users.name.desc(), Users.``id``.asc()).``all``()` `# 11. group by    #根据聚合索引进行二次查询要用having``from` `sqlalchemy.sql ``import` `func` ` ``ret ``=` `session.query(``         ``Users.depart_id,``         ``func.count(Users.``id``),`` ``).group_by(Users.depart_id).``all``()`` ``for` `item ``in` `ret:``         ``print``(item)` ` ``from` `sqlalchemy.sql ``import` `func` ` ``ret ``=` `session.query(``         ``Users.depart_id,``         ``func.count(Users.``id``),`` ``).group_by(Users.depart_id).having(func.count(Users.``id``) >``=` `2``).``all``()`` ``for` `item ``in` `ret:``         ``print``(item)` `# 12.union 和 union all``"""``select id,name from users``UNION         ``select id,name from users;``"""`` ``q1 ``=` `session.query(Users.name).``filter``(Users.``id` `> ``2``)`` ``q2 ``=` `session.query(Favor.caption).``filter``(Favor.nid < ``2``)`` ``ret ``=` `q1.union(q2).``all``()   ``#union上下拼接去重` ` ``q1 ``=` `session.query(Users.name).``filter``(Users.``id` `> ``2``)`` ``q2 ``=` `session.query(Favor.caption).``filter``(Favor.nid < ``2``)`` ``ret ``=` `q1.union_all(q2).``all``()   ``#union_all 上下拼接不去重  ，join左右拼接`
```

crm 风控