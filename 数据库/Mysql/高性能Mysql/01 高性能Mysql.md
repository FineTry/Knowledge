# 高性能Mysql

[TOC]

## 一、Mysql基础架构

### 1. 数据库基础

#### 1.1 锁

1. **mysql锁：**共享锁（shared lock），排他锁（exclusive lock），也叫读锁（read lock），写锁（write lock）

* **按粒度分：**表锁（table lock），行锁（row lock）
* **innerdb的行锁是基于索引加锁的。**



#### 1.2 事务

1. **ACID：** 原子性，一致性，隔离性，持久性

2. **隔离级别：**
   
   * READ UNCOMMINTTED（未提交读）
     * 问题：读到另一个事务未提交的数据。
   * READ COMMINTTED（提交读）
     * 问题：两次执行相同查询，结果不一致。
   * REPEATABLE READ（可重复读）
     * 问题：幻读，当某个事务读某个范围内，另一个事务对这个范围插入了新记录，之前的事务再读取，就会造成幻读。
   
   * SERIALIZABLE（可串行化）
   
3. **死锁：**

   ```sql
   -- 事务1：
   START TRANSACTION;
   UPDATE USER SET AGE = 20 WHERE ID =1;
   UPDATE USER SET AGE = 18 WHERE ID =2;
   COMMIT;
   -- 事务2：
   START TRANSACTION;
   UPDATE USER SET AGE = 21 WHERE ID =2;
   UPDATE USER SET AGE = 31 WHERE ID =1;
   COMMIT;
   ```

   多个事务，同时拿了对面所需要的资源，又去访问对面持有的资源，就会造成死锁。

4. innodb会在执行事务是根据隔离级别，在需要时随时加锁。在commit / rollback时释放所有锁。

5. MVCC（多版本控制器，innodb为了增加读的效率，而做出的优化，具体就是在没行添加一个插入时间（版本号），添加一个删除时间（版本号），注意，update是删了再插入。）在READ COMMINTTED，REPEATABLE READ隔离级别下生效。

   > 使用MVCC可以保证在读的情况下，大部分不需要加锁，提高效率。






## 二、存储引擎

### 1. 存储引擎

1. innodb支持行锁和事务，myisam支持行锁，不支持事务。
2. 如果没有特别必要，不要使用混合引擎。



## 2. 

