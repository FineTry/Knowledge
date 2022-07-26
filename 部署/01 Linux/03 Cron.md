# Cron定时任务

[TOC]

## 一、名词解释：

  cron是服务名称，crond是后台进程，crontab则是定制好的计划任务表。





## 二、软件包安装：

要使用cron服务，先要安装vixie-cron软件包和crontabs软件包，两个软件包作用如下：

vixie-cron软件包是cron的主程序。
crontabs软件包是用来安装、卸装、或列举用来驱动 cron 守护进程的表格的程序。

查看是否安装了cron软件包: rpm -qa|grep vixie-cron

查看是否安装了crontabs软件包:rpm -qa|grep crontabs

 

如果本地没有安装包，在能够连网的情况下可以在线安装

yum install vixie-cron
yum install crontabs



**查看crond服务是否运行：**

/sbin/service crond status

或

ps -elf|grep crond|grep -v "grep"

 



## 三、crond服务操作命令:

/sbin/service crond start //启动服务  
/sbin/service crond stop //关闭服务  
/sbin/service crond restart //重启服务  
/sbin/service crond reload //重新载入配置

 



## 四、配置定时任务：

cron有两个配置文件，一个是一个全局配置文件（/etc/crontab），是针对系统任务的；一组是crontab命令生成的配置文件（/var/spool/cron下的文件），是针对某个用户的.定时任务配置到任意一个中都可以。

查看全局配置文件配置情况: cat /etc/crontab

\---------------------------------------------
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

\# run-parts
01 * * * * root run-parts /etc/cron.hourly
02 4 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly
\----------------------------------------------

查看用户下的定时任务:crontab -l或cat /var/spool/cron/用户名