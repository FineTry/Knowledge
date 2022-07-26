# Docker

[TOC]



## 一、启动docker服务

1. `systemctl start docker`

2. docker开机启动：`systemctl enable docker`

3. 设置容器自动启动

   1. 创建容器时：

      ```
      docker run -d --restart=always --name 设置容器名 使用的镜像
      （上面命令  --name后面两个参数根据实际情况自行修改）
      
       --restart具体参数值详细信息：
             no　　　　　　　　容器退出时，不重启容器；
             on-failure　　  只有在非0状态退出时才重新启动容器；
             always　　　　　 无论退出状态是如何，都重启容器；
      ```

   2. 修改已有容器：

      ```
      docker update --restart=always 容器ID(或者容器名)
      （容器ID或者容器名根据实际情况修改）
      ```

      