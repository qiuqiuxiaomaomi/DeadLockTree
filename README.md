# DeadLockTree
死锁技术研究


<pre>
在数据库中存在两种基本类型的锁：
      1）排它锁（Exclusive Locks，即X锁）
      2）共享锁(Share Locks，即S锁)

      当数据对象被加上排他锁时，其他的事务不能对它读取和修改。加了共享锁的数据对象可以被其他
      事务读取，但不能修改，数据库利用这两种基本的锁类型来对数据库的事务进行并发控制。

Mysql死锁技术研究

      1）用户A锁住了T1，然后又访问表T2,另一个表B锁住了表T2，同时企图访问表T1，这时用户A
        由于B已经锁住了表T2,用户A它必须等待用户B释放表T2的锁才能继续，同样用户B要等待用户A释放T1的锁才能继续，于是产生死锁。

         解决方法：
                 1）一般是代码bug引起，调整程序业务逻辑；         

      2）A查询一条记录，然后修改该记录，用户B修改该条记录，用户A事务里的性质由查询的共享锁
         编程修改的独占锁，而用户B的独占锁由于A有共享锁，所以必须等待A释放共享锁，而由于A
         的共享锁无法上升到独占锁所以不能释放共享锁，于是出现了死锁。

         解决方法：
                 1：使用乐观锁解决方案。乐观锁大多数是基于数据版本Version记录机制实现，
                    即为数据增加一个版本标识，在基于数据表的版本方案中，一般是通过位数据
                    库表增加"version"字段来实现。
                 2：谨慎使用悲观锁控制
                    如 select *** from update ***

      3）如果在事务中执行了一条不满足条件的update语句，则执行全表扫描，把行级锁上升为表级
         锁，多个这样的事务执行后，就很容易产生死锁和阻塞。类似的情况，还有当表中的数据量
         非常庞大而索引建的过少或不合适的情况，使得经常发生全表扫描，最终应用系统会越来越慢
         ，最终发生阻塞或死锁。

         解决方法：
                 SQL语句中不要使用太复杂的关联多表查询，使用执行计划对SQL语句进行分析，
         对于有全表扫描的SQL语句，建立相应的索引进行优化。
</pre>