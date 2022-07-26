# Docker 常用命令

[TOC]



## 一、Docker生命周期命令

### 1. docker run

> description：创建一个新的容器。

**语法：**`docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

* 常用OPTIONS说明：
  * **-it:** 分配一个伪终端，并且可以交互
  * **-d:** **后台**启动
  * **-P:** 随机端口映射，容器内部端口**随机**映射到主机的高端口
  * **-p:** 指定端口映射，格式为：**主机(宿主)端口:容器端口**
  * **--name="nginx-lb":** 为容器指定一个名称；
  * **-e username="ritchie":** 设置环境变量；
  * **--net="bridge":** 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
    * bridge：桥接模式，默认，分配一个单独网络。
    * host：共享主机网络。
    * none：不配置网络。
    * container：多容器共享一个网络，常用于和K8S。
  * **--volume , -v:** 绑定一个卷
  * **--restart="no"**： 指定容器停止后的重启策略:
    - no：容器退出时不重启
    - on-failure：容器故障退出（返回值非零）时重启
    - always：容器退出时总是重启

**例子：**`docker run -itd --name=my-nginx --net=host --restart=always -p 1111:1111 <容器id>`



### 2. docker start/stop/restart

* **例子：**
  * `docker start <容器id>` 	启动容器
  * `docker stop <容器id>`       停止容器
  * `docker restart <容器id>`  重启容器



### 3. docker rm/rmi

* `docker rm -f <容器id>`  强制删除一个容器
* `docker rmi -f <镜像id>` 强制删除一个镜像
* **删除所有停止运行的容器：**`docker container prune`
* **删除所有未被容器使用的镜像：**`docker image prune -a`



### 4. docker exec

* 进入容器：`docker exec -it <容器id> /bin/bash`



### 5. docker ps/images/port

* **查看所有容器：** `docker ps -a`
* **查看所有镜像：** `docker images -a`
* **查看端口映射：**`docker port <容器id>`



### 6. docker logs

* **实时跟踪日志：** `docker logs <容器id> -f` 
* **查看最新100行日志，并且实时跟踪日志：** `docker logs <容器id> --tail=100 -f`



### 7. docker commit/cp

* **容器生成镜像：** `docker commit -a "提交作者" -m "提交说明" <容器id> <镜像名>` 
* **主机拷贝文件到容器：**`docker cp <主机文件/目录> <容器id>:<容器目录>`
* **容器拷贝文件到主机：**`docker cp <容器id>:<容器目录> <主机文件/目录>`



### 8. docker login/logout

* **登录镜像仓库：** `docker loin `  /   `docker login -u 用户名 -p 密码`
* **登出镜像仓库：** `docker logout`



### 9. docker pull/push

* **拉取镜像：** `docker pull <镜像>`
* **上传镜像：** `docker push <本地镜像>`



### 10. doker tag

* **标记镜像，将其归入某一仓库：** `docker tag ubuntu:15.10 <name/镜像在仓库中位置>:<标签> `



### 11. docker info/version

> 查看docker一些信息



### 12. docker build/save/load

1. **构建镜像：** `docker build [OPTIONS]`
   * -f :指定Dockerfile路径，不指定，默认找当前路径
   * -t :镜像的名字以及标签，name:tag
   * 例：`docker build -t runoob/ubuntu:v1 -f /path/Dockerfile .` 
2. **保存镜像为tar：**`docker save [OPTIONS]`
   * -o :输出到的文件
   * 例：`docker save -o [输出名] [镜像名]`
3. **加载镜像：**`docker load [OPTIONS]`
   1. -i :指定导入文件
   2. 例：`docker load < busybox.tar.gz`



### 13. docker update

> 修改容器信息。

* `docker update [OPTIONS] CONTAINER [CONTAINER...]`

* OPTIONS：
  * --restart：当容器退出时重新启动的策略
  * --memory-swap：交换空间，“-1”允许无限交换.
  * --memory , -m：内存限制

例子：`docker update --restart=always abebf7571666`