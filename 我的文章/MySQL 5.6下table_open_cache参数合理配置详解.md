# [MySQL 5.6下table_open_cache参数合理配置详解](https://www.cnblogs.com/fjping0606/p/6531292.html)



table_open_cache指定表高速缓存的大小。每当MySQL访问一个表时，如果在表缓冲区中还有空间，该表就被打开并放入其中，这样可以更快地访问表内容。
通过检查峰值时间的状态值Open_tables和Opened_tables，可以决定是否需要增加table_open_cache的值。
如果你发现**open_tables**等于**table_open_cache**，并且**opened_tables**在不断增长，那么你就需要增加table_open_cache的值了(上述状态值可通过SHOW GLOBAL STATUS LIKE ‘Open%tables’获得)。
注意，不能盲目地把table_open_cache设置成很大的值，设置太大超过了shell的文件描述符(通过ulimit -n查看)，造成文件描述符不足，从而造成性能不稳定或者连接失败。

测试环境：腾讯云CDB，内存4000M，在控制台查看到table_open_cache＝512，监测table_open_cache设置是否合理，是否需要优化。

![img](https://images2015.cnblogs.com/blog/743229/201703/743229-20170310161408139-418833534.jpg)

![img](https://images2015.cnblogs.com/blog/743229/201703/743229-20170310161551186-593451714.jpg)

发现open_tables等于table_open_cache，都是512，说明mysql正在将缓存的表释放以容纳新的表，此时可能需要加大table_open_cache的值，4G内存的机器，建议设置为2048
比较适合的值：
Open_tables / Opened_tables >= 0.85
Open_tables / table_open_cache <= 0.95
如果对此参数的把握不是很准，有个很保守的设置建议：把MySQL数据库放在生产环境中试运行一段时间，然后把参数的值调整得比Opened_tables的数值大一些，并且保证在比较高负载的极端条件下依然比Opened_tables略大。