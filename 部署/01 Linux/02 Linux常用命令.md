# Linux常用命令

[TOC]


## 一、常用命令介绍

#### 1. 改变主机名

* **`sudo hostnamectl set-hostname <newhostname>`**

#### 2. linux之间互传文件
* **互传文件：`scp <file> root@192.168.1.26:<传入路径>`**
* **互传文件夹：`scp -r <文件夹> root@192.168.1.26:<传入路径>`**

#### 3. win和Linux之间互传文件

##### 3.1 使用`lrzsz`

* 安装`lrzsz`：`yum -y install lrzsz`
* **`sz`：** 上传　 **`rz：`** 下载

##### 3.2 使用sftp文件传输

#### 4. 查看软件安装路径：`whereis java`

#### 5. 启动配置文件：`source`
* 重启配置文件：`source <newFile>`  例子：source /etc/profile

#### 6. 显示或者修改时间：`date`

* 修改时间：date -s 2019-10-1（修改年月日）   date -s 10:00（修改时分）  clock -w（保存修改时间）
* 格式化显示时间：`date '+%Y-%m-%d %H:%M:%S'`





## 二、文件管理

#### 1. `ll`和`ls`，`pwd`，`cd`，`clear`，`vi`和`vim` 等常用命令就不做详细介绍。

* 查看所有：`ls -a`
* 列出指定目录root：`ls -l /root`

#### 2. 创建文件夹/文件

* 创建文件夹： **`mkdir  <newFile>`**
* 创建文件：**`touch <File>`**

#### 3. 查看和编辑文件夹/文件

* 编辑文件：`vi或vim`
* 查看文件：`cat <filename>`&emsp;标明行号： `cat -Ab <filename>`
* 实时查看文件：`tail -f <filename>`
* 分页查看：`more -c -10 <filename>`
  * cat 由第一行开始显示文件内容
  * tac 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
  * nl  显示的时候，顺道输出行号！
  * more 一页一页的显示文件内容
  * less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
  * head 只看头几行
  * tail 只看尾巴几行

#### 4. 删除文件夹/文件：`rm -rf`

#### 5. 复制，移动，命名文件夹/文件：`cp`和`mv`

* 将test1目录复制到test2目录：`cp -r /mydata/tes1 /mydata/test2`
* 移动或覆盖文件：`mv text.txt text2.txt`

#### 6. 改变文件访问权限：`chmod`

* 改变当前文件的访问权限：`chmod 777 <file>`
* 递归改变该文件和下面所有文件权限：`chmod -R 777 <file>`

#### 7. 设置用户密码：`passwd root`

#### 8. 切换用户：`su`

* 切换超级用户：`su -`

#### 9. 显示指定命令的帮助：`man ls`

#### 10. 显示使用者：`who`

- 显示目前登录到系统的用户：`who -buT`

#### 11. 显示系统内存状态（单位MB）：`free -m`

#### 12. 进程管理命令：`ps` 和`kill`

* 查看含xxx的进程：**`ps -ef |grep <xxx>`**
* 杀死进程：**`kill -9 <进程id>`**

#### 13. 查看即时活跃的进程，类似Windows的任务管理器：`top`





## 三、服务管理

> 服务管理有两种：systemctl和service，systemctl集成了service和chkconfig的所有功能，以后使用systemctl。

### 1. 系统服务管理：`systemctl`

> 以下将以防火墙firewalld服务举例。

#### 1.1 系统中各个服务的状态：

* ```linux
  systemctl list-units --type=service
  ```

#### 1.2 服务的查看，关闭，启动...

* **查看状态：**`systemctl status firewalld`
* **关闭服务：**`systemctl stop firewalld`
* **启动服务：**`systemctl start firewalld`
* **重新启动服务**（不管当前服务是启动还是关闭）：`systemctl restart firewalld`
* **重新载入配置信息而不中断服务：**`systemctl reload firewalld`
* **禁止服务开机自启动：**`systemctl disable firewalld`
* **设置服务开机自启动：**`systemctl enable firewalld`



#### 

## 四、磁盘和网络管理

#### 1. 查看文件占用情况

* 所有文件占用情况： **`df -h [可以指定路径]`**
* 当前目录下文件占用情况： **`du -lh --max-depth=1 [可以指定路径]`**

#### 2. 查看ip情况：`ip addr`  和 `ifconfig`

#### 3. 查看网络连接情况：`netstat`

- 查看当前路由信息：`netstat -rn`
- 查看所有有效TCP连接：`netstat -an`

- 查看系统中启动的监听服务：`netstat -tulnp`
- 查看处于连接状态的系统资源信息：`netstat -atunp`

#### 4. 从网络上下载文件：`wget`





## 五、压缩与解压：`tar`

- 将/etc文件夹中的文件归档到文件etc.tar（并不会进行压缩）：

```
tar -cvf /mydata/etc.tar /etc
```

- 用gzip压缩文件夹/etc中的文件到文件etc.tar.gz：

```
tar -zcvf /mydata/etc.tar.gz /etc
```

- 用bzip2压缩文件夹/etc到文件/etc.tar.bz2：

```
tar -jcvf /mydata/etc.tar.bz2 /etc
```

- 分页查看压缩包中内容（gzip）：

```
tar -ztvf /mydata/etc.tar.gz |more -c -10
```

- 解压文件到当前目录（gzip）：

```
tar -zxvf /mydata/etc.tar.gz
```





## 六、软件的安装与管理

### 1. rpm

- 安装软件包：rpm -ivh nginx-1.12.2-2.el7.x86_64.rpm
- 模糊搜索软件包：rpm -qa | grep nginx
- 精确查找软件包：rpm -qa nginx
- 查询软件包的安装路径：rpm -ql nginx-1.12.2-2.el7.x86_64
- 查看软件包的概要信息：rpm -qi nginx-1.12.2-2.el7.x86_64
- 验证软件包内容和安装文件是否一致：rpm -V nginx-1.12.2-2.el7.x86_64
- 更新软件包：rpm -Uvh nginx-1.12.2-2.el7.x86_64
- 删除软件包：rpm -e nginx-1.12.2-2.el7.x86_64

### 2. yum

- 安装软件包： yum install nginx
- 检查可以更新的软件包：yum check-update
- 更新指定的软件包：yum update nginx
- 在资源库中查找软件包信息：yum info nginx*
- 列出已经安装的所有软件包：yum info installed
- 列出软件包名称：yum list nginx*
- 模糊搜索软件包：yum search nginx

### 3. linux安装软件：`rpm 和 yum`   rpm是一个软件包管理器，可以安装软件包。

* yum可以从网上下载安装包，并且可以自动下载依赖包。 
* 安装rpm文件可以使用yum，自动下载依赖。
* yum install xxx.rpm
