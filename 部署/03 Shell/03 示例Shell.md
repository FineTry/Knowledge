# 示例Shell

[TOC]

## 一、Shell示例



### 1. 定时打包

```shell
# 注意 date 命令需要使用反引号括起来,反引号在键盘<tab>键上面
tar	-czf	log-`date +%Y%m%d`.tar.gz	/var/log 
# crontab ‐e	#编写计划任务,执行备份脚本，每分钟执行一次
*	*	*	*	*	/root/logbak.sh
```



### 2. 监控内存和磁盘容量

```shell
#!/bin/bash
# 实时监控本机内存和硬盘剩余空间,剩余内存小于500M、根分区剩余空间小于1000M时,发送报警邮件给root管理员
# 提取根分区剩余空间
disk_size=$(df / | awk '/\//{print $4}')
# 提取内存剩余空间
mem_size=$(free | awk '/Mem/{print $4}')
while :
do
# 注意内存和磁盘提取的空间大小都是以 Kb 为单位
if  [  $disk_size -le 512000 -a $mem_size -le 1024000  ]
then
    mail  ‐s  "Warning"  root  <<EOF
	Insufficient resources,资源不足
EOF
fi
done
```



### 3. 根据用户输入的密码生成账号

```shell
#!/bin/bash
# 编写脚本:提示用户输入用户名和密码,脚本自动创建相应的账户及配置密码。如果用户
# 不输入账户名,则提示必须输入账户名并退出脚本;如果用户不输入密码,则统一使用默
# 认的 123456 作为默认密码。

read -p "请输入用户名: " user
#使用‐z 可以判断一个变量是否为空,如果为空,提示用户必须输入账户名,并退出脚本,退出码为 2
#没有输入用户名脚本退出后,使用$?查看的返回码为 2
if [ -z $user ];then
   	echo "您不需输入账户名"
 	exit 2
fi
#使用 stty ‐echo 关闭 shell 的回显功能
#使用 stty  echo 打开 shell 的回显功能
stty -echo
read -p "请输入密码: " pass
stty echo
pass=${pass:‐123456}
useradd "$user"
echo "$pass" | passwd ‐‐stdin "$user"
```

