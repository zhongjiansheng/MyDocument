## Myql

#### 锁策略

* 表锁

  对一张表进行锁定，并且 写操作可以插入到读操作前面。在面对**ALTER、TABLE**之类的语句使用表锁，忽略存储引擎的锁机制。

* 行级锁

  最大程度支持并发处理（**开销极大**）,只能在存储引擎层实现，MySQL服务器层没有实现

* ANSI SQL隔离级别

  ![image-20191126152736450](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\image-20191126152736450.png)

* 设置隔离级别

  ![image-20191126153747327](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\image-20191126153747327.png)

**注意:**如果事务中没有禁用AUTOCOMMIT，则不能同时使用LOCK、TABLES



#### InnoDB引擎(基于表的，而不是数据库)

* MVCC

  通过在每行记录后买你保存两个隐藏的列来实现:行的创建时间、行的过期时间。

* MVCC工作的隔离级别

  REPEATABLE READ 和 READ COMMITTED

  > RERAD UNCOMMITTED总是读取最新的数据行
  >
  > SERIALIZABLE则对所有读取的行都加锁



#### MyISAM引擎

* 特性

  1. 加锁与并发

     对整张表加锁，不同于InnoDB可以加行锁。读取时加共享锁，写入时加入排他锁

  2. 修复

     通过手工或者自动执行检查和修复操作。

     > CHECK TABLE mytable
     >
     > REPAIR TABLE mytable

  3. 索引特性

  4. 延迟更新索引键

     指定了DELAY_KEY_WRITE选项后，修改后的索引数据会写到内存中的键缓冲区，只有在清理缓冲区或者关闭表的时候才会写入到磁盘。

     > 好处：提升了写入性能
     >
     > 缺点：数据库或者主机崩溃时会造成索引损坏

* MyISAM压缩表

  当某个表只作为读取的时候，可以进行压缩，节省磁盘，提高查询性能



#### 转换表的引擎

* ALTER TABLE

  ![image-20191126164816507](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\image-20191126164816507.png)

* 导出与导入

  利用mysqldump工具

* 创建与查询(CREATE 和 SELECT)

  ![image-20191126165111782](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\image-20191126165111782.png)

#### 服务器性能剖析

* 剖析单条查询

  1. 使用SHOW PROFILE

     > 1.SET profiling = 1;(开启)
     >
     > 2.执行sql语句
     >
     > 3.SHOW PROFILES;
     >
     > 4.SHOW PROFILE FOR QUERY 133;(根据Id显示详细数据)