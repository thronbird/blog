---

title: InnoDb RR隔离级别需要注意的一个坑

---

## 问题

在并发更新同一行记录场景下（比如调用下游系统可能返回同步结果和异步结果（mq消息），目前更新订单表是通过乐观锁（根据订单原状态做判断）做并发控制）：

开启事务-》更新订单A-》判断update返回值是否>0？继续下面的流程：否则重新查询订单A的状态看是否已经被其他线程更新-如果已经更新到目标状态则正常返回，否则抛出异常

在批量更新的时候，可能出现异常情况：

开启事务-》更新订单A-》判断update返回值是否>0？更新订单B（-》更新订单C..）：否则重新查询订单A|B|C的状态看是否已经被其他线程更新-如果已经更新到目标状态则正常返回，否则抛出异常

可能更新订单B|C的情形下查出的结果有时是原状态但是数据库实际上已经更新到目标状态

## 实验

在console1：

```sql
BEGIN;
select status from Order where id = 'A';
SELECT SLEEP(10);
SELECT  status from Order where id = 'B';
COMMIT
```

console1的sleep期间在console2执行：

```sql
BEGIN;
UPDATE Order SET `status` = '失败' where id = 'B'and `status` = '处理中';
COMMIT;
```

可看到结果是处理中而非console2执行之后的失败。

## 原因

InnoDb 默认隔离级别是RR，在此级别下，开启事务之后做select属于[Consistent Nonlocking Reads](<https://dev.mysql.com/doc/refman/8.0/en/innodb-consistent-read.html>)，(而update、select for update这类语句属于 [locking read](<https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-reads.html>)),



**RR级别的情况下，开启事务，如果订单A（有并发更新，并）做了select查询那么订单B做select查询的时候的结果仍然是订单A做select查询那一刻的结果**

**rc每次快照读时都会去更新事务id数组，rr只在一开始时创建数组**。

## Reference

[Mysql可重复读](<https://zhuanlan.zhihu.com/p/55872397>)

[Transaction Isolation Levels](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html)

[Consistent Nonlocking Reads](http://www.mybatis.org/mybatis-3/configuration.html#typeHandlers)

 [locking read](<https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-reads.html>)