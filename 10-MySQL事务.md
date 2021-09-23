<!-- order:10 -->
## MySQL 事务

MySQL 事务主要用于处理操作量大，复杂度高的数据。比如说，在人员管理系统中，你删除一个人员，你即需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务！

- 在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。
- 事务处理可以用来维护数据库的完整性，保证成批的SQL语句要么全部执行，要么全部不执行。
- 事务用来管理 insert , update , delete 语句。

一般来说，事务是必须满足4个条件（ACID）： Atomicity（原子性或不可分割性）、Consistency（一致性）、Isolation（隔离性或独立性）、Durability（持久性）

- 1、**原子性：**一组事务，要么成功；要么撤回，即事务在执行过程中出错会回滚到事务开始前的状态。
- 2、**一致性** ： 一个事务不论是开始前还是结束后，数据库的完整性都没有被破坏。因此写入的数据必须完全符合所有预设规则（资料精确度、串联性以及后续数据库能够自发完成预定工作）。
- 3、**隔离性：**数据库允许多个事务并发的同时对其数据进行读写修改等操作，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离可分为：Read uncommitted（读未提交）、Read committed（读提交）、Repeatable read（可重复读）、Serializable（串行化）。
- 4、持久**性：**事务在处理结束后对数据做出的修改是永久的，无法丢失。

### 事务控制语句

1，显式的开始一个事务：

```sql
start transaction
```

或

```sql
begin
```

2, 做保存点，一个事务中可以有多个保存点：

```sql
savepoint 保存点名称
```

3, 提交事务，并使数据库中进行的所有修改成为永久性的：

```sql
commit
```

或

```sql
commit work
```

4，回滚结束用户的事务，并撤销正在进行的所有未提交的修改：

```sql
rollback
```

或

```sql
rollback work
```

5.删除一个事务的保存点，若没有指定保存点，执行该语句操作会抛错。

```sql
release savepoint 保存点名称
```

6.将事务滚回标记点：

```sql
rollback to 标记点
```

7.设置事务的隔离级别。InnoDB 存储引擎提供事务的隔离级别有READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ 和 SERIALIZABLE。

```sql
set transaction
```

### 事务处理方法

1.用 begin ， rollback ， commit 来实现事务处理。

2.用 set 来改变 MySQL 的自动提交模式。

- set autocommit = 0 （禁止自动提交）。
- set autocommit = 1 （开启自动提交）。


