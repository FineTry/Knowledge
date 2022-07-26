# Shell启动脚本

[TOC]

## 一、删除对应服务，启动

```shell
#!/bin/bash
PLATFROM_API=$(ps -ef | grep platform-api | grep -v grep|awk '{print $2}')
kill -s 9 ${PLATFROM_API}
echo "--------platform-api 停止成功--------------"
nohup java -jar /home/edu/platform-api-0.0.1-SNAPSHOT.jar >/home/edu/platform.log & 
echo "--------platform-api 启动成功，日志输出：/home/platform.log--------------"
```





## 二、第二种

```shell
#!/bin/bash

export EUREKA_JAR=eureka/eureka-server-0.0.1-SNAPSHOT.jar
export GATEWARY_JAR=gateway/gateway-server-0.0.1-SNAPSHOT.jar
export SSOSERVER_JAR=ssoserver/pl-sso-server-0.0.1-SNAPSHOT.jar


export EUREKA_port=8081
export GATEWAY_port=8082
export SSOSERVER_port=8083

case "$1" in

start)
        ## 启动eureka
        echo "--------eureka 开始启动--------------"
        nohup java -jar $EUREKA_JAR >/dev/null 2>&1 &
        EUREKA_pid=`lsof -i:$EUREKA_port|grep "LISTEN"|awk '{print $2}'`
        until [ -n "$EUREKA_pid" ]
            do
              EUREKA_pid=`lsof -i:$EUREKA_port|grep "LISTEN"|awk '{print $2}'`  
            done
        echo "EUREKA_JAR pid is $EUREKA_pid" 
        echo "--------eureka 启动成功--------------"

        ## 启动gateway
        echo "--------开始启动GATEWAY---------------"
        nohup java -jar $GATEWARY_JAR >/dev/null 2>&1 &
        GATEWAY_pid=`lsof -i:$GATEWAY_port|grep "LISTEN"|awk '{print $2}'`
        until [ -n "$GATEWAY_pid" ]
            do
              GATEWAY_pid=`lsof -i:$GATEWAY_port|grep "LISTEN"|awk '{print $2}'`  
            done
        echo "GATEWARY_JAR pid is $GATEWAY_pid"    
        echo "---------GATEWAY 启动成功-----------"
		
		        ## 启动config
        echo "--------开始启动CONFIG---------------"
        nohup java -jar $SSOSERVER_JAR >/dev/null 2>&1 &
        CONFIG_pid=`lsof -i:$CONFIG_port|grep "LISTEN"|awk '{print $2}'` 
        until [ -n "$CONFIG_pid" ]
            do
              CONFIG_pid=`lsof -i:$CONFIG_port|grep "LISTEN"|awk '{print $2}'`  
            done
        echo "SSOSERVER_JAR pid is $CONFIG_pid"     
        echo "---------CONFIG 启动成功-----------"

```





## 三、第三种

```shell
#!/bin/bash
if [ -x "$JAVA_HOME/bin/java" ]; then
JAVA="$JAVA_HOME/bin/java"
else
JAVA=`which java`
fi
if [ ! -x "$JAVA" ]; then
echo "Could not find any executable java binary. Please install java in your PATH or set JAVA_HOME"
exit 1
fi
JAVA_OPTS="-server -Xms1024m -Xmx2048m"
EUREKA_JAR="eureka/eureka-server-0.0.1-SNAPSHOT.jar" 
GATEWARY_JAR="gateway/gateway-server-0.0.1-SNAPSHOT.jar" 
SSOSERVER_JAR="ssoserver/pl-sso-server-0.0.1-SNAPSHOT.jar" 
BLOODSERVER_JAR="bloodserver/pl-blood-server-0.0.1-SNAPSHOT.jar" 
BLOODDBSERVER_JAR="blooddbserver/pl-blood-db-server-0.0.1-SNAPSHOT.jar" 
nohup $JAVA $JAVA_OPTS -jar $EUREKA_JAR >/dev/null 2>&1 &
echo "--------eureka 启动成功--------------" 
nohup $JAVA $JAVA_OPTS -jar $GATEWARY_JAR >/dev/null 2>&1 &
echo "--------gateway 启动成功--------------"
nohup $JAVA $JAVA_OPTS -jar $SSOSERVER_JAR >/dev/null 2>&1 &
echo "--------ssoserver 启动成功--------------"
nohup $JAVA $JAVA_OPTS -jar $BLOODSERVER_JAR >/dev/null 2>&1 &
echo "--------bloodserver 启动成功--------------"
nohup $JAVA $JAVA_OPTS -jar $BLOODDBSERVER_JAR >/dev/null 2>&1 &
echo "--------blooddbserver 启动成功--------------"
```

