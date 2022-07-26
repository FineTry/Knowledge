# kubernates命令集

1. 查看所有命名空间
   1. kubectl get namespaces
2. 使用yaml启动deployment
   1. kubectl apply -f <xxx.ymal>
3. 查看所有的deployment
   1. kubectl get deployment --namespace = <namespace>
4. 删除deployment，pod，service
   1. kubectl delete deployment <deployment > --namespace=<namespace>
5. 查看日志
   1. kubectl logs <pod/deployment > --namespace=dg-app-test  -f 
6. 重启kubernetes
   1. kubectl replace --force -f xxxx.yaml
7. 修改配置文件
   1. kubectl edit pod <pod> -n <namespace>
8. 查看所有
   1. kubectl get all -n <namespace>
   2. kubectl get all -n <namespace> -o wide



```
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -p 5433:5432 -d postgres
docker run -itd --rm -v pgdata:/var/lib/postgresql/data postgres:9.6
```



docker ps -l  查看全部镜像包括停止

docker rm <容器id>  删除关闭的容器

docker stop <容器id> 停止容器

**docker cp 39d4396197558:/usr/local/tomcat/webapps/jpress.war .**

* **39d4396197558** 是指容器的ID或者名称

* **/usr/local/tomcat/webapps/jpress.war** 是文件在容器的位置

* **.** 表示拷贝到当前目录下