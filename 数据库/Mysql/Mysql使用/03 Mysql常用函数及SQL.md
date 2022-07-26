# Mysql常用函数及SQL

[TOC]

## 一、Mysql常用函数

### 1. exists

> EXISTS用于检查子查询是否至少会返回一行数据，该子查询实际上并不返回任何数据，而是返回值True或False。

* 用法实例：

### 2. 时间处理

```sql
select concat(left(now(), 14),'00:00') - INTERVAL 1 DAY
-- 获取当前1天前的起始时间
-- INTERVAL 1 DAY可以结合date_add()和date_sub()使用
select CURRENT_DATE  -- 2021-06-02
select CURRENT_DATE+0  -- 20210602 转化为数字
select now()  -- 获取当前时间
```

### 3. 插入or更新

```sql
-- 插入数据ON DUPLICATE KEY UPDATE 为当主键存在时更新，不存在添加
insert into daily_hit_counter(day, slot, cnt)
	VALUES(CURRENT_DATE, RAND()*100,1)
	ON DUPLICATE KEY UPDATE cnt = cnt +1;
```







## 二、Mysql常用SQL

1. 给数据加锁

```sql
-- 给数据加锁
select…for update
```





```sql
-- 行转列，转为中文
SELECT
	(
SELECT
	GROUP_CONCAT( d.VALUE ) 
FROM
	(
SELECT DISTINCT
	SUBSTRING_INDEX( SUBSTRING_INDEX( a.NAME, ';', b.help_topic_id + 1 ), ';',- 1 ) NAME,
SUBSTRING_INDEX( SUBSTRING_INDEX( a.VALUE, ';', b.help_topic_id + 1 ), ';',- 1 ) 
VALUE
	
FROM
	( SELECT 'J;P;G;E' NAME, '早上;晚上;清晨;午夜' VALUE FROM DUAL ) a
JOIN mysql.help_topic b ON b.help_topic_id < ( LENGTH( a.NAME ) - LENGTH( REPLACE ( a.NAME, ';', '' ) ) + 1 ) 
) d 
WHERE
	FIND_IN_SET( NAME, c.name ) 
	) label 
FROM
	( SELECT REPLACE ( NAME, ';', ',' ) NAME FROM test ) c
```

