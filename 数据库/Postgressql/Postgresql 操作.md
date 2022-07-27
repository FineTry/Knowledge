# Postgresql 操作：

\1. Psql两种连接，

(1)   本机：su postgres;  psql 就可以进入psql超级管理员

![img](https://github.com/FineTry/Picture/raw/DEV/knowledge/clip_image002.jpg)

(2)   采用psql控制台连接： 以川大服务器为例，进入postgres数据库，用户名为：postgres，密码：shufeng

①  psql -h 10.3.0.93 -p 5432 -U postgres -d postgres;

![img](https://github.com/FineTry/Picture/raw/DEV/knowledge/clip_image004.jpg)

当我们看到psotgres=#代表我们拥有了超级权限。

 

\2. 数据库内部操作：

(1)   创建数据库：create database <dbname>;

(2)   删除数据库：drop database <dbname>;

(3)   删除数据库可能存在连接未断开情况，执行以下，关闭所有连接：

①  SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity WHERE datname='db_eduplatform' AND pid<>pg_backend_pid();

(4)   修改数据库所属用户：alter database <dbname> owner to shufeng;

(5)   查看所有数据库：\l

(6)   切换数据库：\c <dbname>

(7)   切换用户：\c - <name>

(8)   显示所有用户：\du

 

\3. 拷贝数据库备份：我们采用psql的pg_dump，注意这种备份并不需要进入psql内部。

(1)   pg_dump -h 192.168.1.75 -p 5432 -U shufeng -d db_eduplatform2 -n public >db_edu4.sql

①  我们拆解一下，

②  -h 指定ip，

③  -p 指定端口，

④  -U 指定拷贝数据库的用户名，

⑤  -d 指定需要拷贝的数据库名，

⑥  -n 指定模式，我们这里指定public。 

⑦  > 后面代表输出位置。

![img](D:\个人文件\学无止境\Image\clip_image006.jpg)

 

\4. 恢复数据：psql -h 10.3.0.93 -p 5432 -U shufeng -d db_eduplatform2 < db_edu4.sql，具体属性参照上面。