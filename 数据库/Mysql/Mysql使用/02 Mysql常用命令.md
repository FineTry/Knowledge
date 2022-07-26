# Mysql常用命令

[TOC]

## 一、常用mysql命令

### 1. 自动提交

```SQL
mysql> SHOW VARIABLES LIKE 'AUTOCOMMIT';
```

### 2. 隔离级别

```SQL
mysql> SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

> 只会改变当前会话的隔离级别，如要改所有，请修改配置文件。

1. **新进连接要使事务隔离级别生效, 在不重启mysql服务情况下在客户端执行:**

例： set global.tx_isolation='READ-COMMITTED';

tips：设置后新的连接就会使用该隔离级别, 但mysql重启后恢复默认隔离级别Repeatable Read

验证：select @@tx_isolation;

2. **重启也要生效要在mysql配置文件my.cnf中[mysqld]段下加上:**

```
transaction-isolation=Read-Committed
```

### 3. 修改表的存储引擎

```sql
sql> create table myisam_table like my_info;
sql> alter table myisam_table ENGINE = myisam;
sql> insert INTO myisam_table select * from my_info
```

