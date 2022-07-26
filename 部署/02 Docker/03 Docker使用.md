# Docker 使用

[TOC]



## 一、docker 数据管理-数据库容器化并持久化

> 在生产环境中，对应数据库一类，往往需要对数据进行持久化，或者多个容器共享，这就涉及容器数据管理。

* 解决办法：
  * 数据卷：容器内数据映射到本地主机。
  * 数据卷容器：使用特点容器维护数据卷。



### 1. 数据卷

> 数据卷是一个可供容器使用的特殊目录，它将主机操作系统目录直接映射进容器，类似于Linux中的mount操作。

* 数据卷特性：
  * 容器之间共享。
  * 对数据卷的数据修改会马上生效。
  * 卷可以持久化保存，删除容器不影响。

1. 创建本地卷
   * `docker volume create <卷目录名>`
   * **查找卷：** `find / -name <卷目录名>` 
2. 启动容器，加载卷（postgresql为例）
   * `docker run -itd -v <卷目录名>:/var/lib/postgresql/data -p 5433:5432 postgres:9.6`



### 2. 数据卷容器





## 二、deamon.json

> docker V1.12后，可以创建deamon.json对docker进行配置。里面涵盖了几乎所有的启动命令参数。
>
> docker均会默认读取该json。
>
> 路径为：`/etc/docker/daemon.json`



示例：

```
{
        "storage-driver": "overlay2",
        "registry-mirrors": [
                "https://docker.mirrors.ustc.edu.cn",
                "https://registry.docker-cn.com"
        ],
        "log-driver": "json-file",
        "log-opts": {
                "max-size": "20m",
                "max-file": "3"
        }
}
```

* registry-mirrors：#镜像加速的地址，增加后在 docker info中可查看。
* insecure-registries：#配置docker的私库地址

